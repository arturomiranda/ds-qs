# FASE 4 — Seguridad y Validaciones
> Módulo: **Registro de Instancias Velneo + Usuario**
> Estrategia: Defensa en profundidad — 5 capas de seguridad

---

## Rate Limiting por Endpoint

| Endpoint | Límite | Ventana | Acción al exceder |
|---|---|---|---|
| `POST /registration/initiate` | **5 req** | 15 min por IP | `429` + log de IP |
| `POST /registration/verify` | **10 req** | 15 min por IP | `429` + bloqueo temporal |
| `POST /auth/login` | **10 req** | 5 min por IP | `429` + alerta |
| `GET /provisioning/status/:id` | **60 req** | 1 min por usuario | `429` |
| `POST /provisioning/retry/:id` | **3 req** | 1 hora por admin | `429` |

> 🔴 **Sin rate limiting en `/verify`** (estado actual), un atacante puede fuerza bruta el OTP de 6 dígitos. Con límite de 10 intentos / 15 min la ventana de ataque se vuelve inviable.

---

## Schemas de Validación (DTOs)

### `InitiateRegistrationDto`
```
firstName   : string | trim | min:2  | max:50  | no números
lastNameP   : string | trim | min:2  | max:50  | no números
lastNameM   : string | trim | opcional | max:50
email       : string | email RFC5321 | lowercase | max:254
phone       : string | opcional | regex E.164: /^\+[1-9]\d{7,14}$/
company     : string | opcional | max:100 | strip HTML tags
rfc         : string | opcional | regex: /^[A-ZÑ&]{3,4}\d{6}[A-Z0-9]{3}$/i
```

### `VerifyEmailDto`
```
email             : string | email | requerido
code              : string | requerido | exactamente 6 dígitos: /^\d{6}$/
registrationToken : string | requerido | formato JWT: 3 segmentos base64
```

---

## Flujo de Seguridad del registrationToken (JWT Stateless)

El OTP y el `autoPassword` viajan cifrados dentro del JWT — nunca como campos separados.
El cliente solo recibe el token opaco, no puede ver su contenido sin la clave secreta.

**TTLs diferenciados:**
- `registrationToken`: **15 min** — solo para completar el flujo de OTP
- `accessToken`: **8 h** — sesiones activas post-login

> 🔴 **Vulnerabilidad identificada:** El JWT está firmado (HS256) pero **no cifrado**. El `autoPassword` en el payload puede ser decodificado en base64 por cualquiera que intercepte el token.
>
> **Solución propuesta:** Usar **JWE (JWT cifrado)** con `jose` library, o eliminar `plainPassword` del payload y regenerarlo desde una semilla determinista durante el `/verify`.

---

## Aislamiento Multi-Tenant

El `accessToken` post-login incluye `apiUrl` del tenant. El middleware `authenticate` extrae este valor e inyecta `req.tenant.apiUrl` para que los servicios de dominio nunca usen la URL global de variables de entorno.

**Payload del accessToken:**
```
userId    : number
email     : string
username  : string
role      : 'user' | 'admin'
apiUrl    : 'https://tenant.velneo.com/api/v1'
```

---

## Protección de Credenciales Velneo Cloud

| Secreto | Almacenamiento | Acceso permitido |
|---|---|---|
| `VELNEO_CLOUD_EMAIL` | `.env` servidor | Solo `SessionKeeper` |
| `VELNEO_VSERVER_USER` | `.env` servidor | Solo `SessionKeeper` |
| `VELNEO_VSERVER_PASSWORD` | `.env` servidor | Solo `SessionKeeper` |
| `sessionToken` (activo) | **Redis con TTL** | Solo workers internos |
| `autoPassword` usuario | JWT temporal 15min | No persiste en DB en claro |
| `hashedPassword` | MySQL | Nunca retornado en API |

> 🔴 **Recomendación:** El `sessionToken` de Velneo en Redis debe almacenarse **cifrado** (AES-256) con clave `REDIS_ENCRYPTION_KEY`. Si alguien accede a Redis, no obtiene un token válido de Velneo Cloud directamente.

---

## Checklist de Seguridad

```
Transporte
  ✅ HTTPS forzado en producción
  ✅ CORS whitelist configurada
  ✅ Helmet.js activo

Entrada
  ✅ DTO con schema Zod/Joi en cada endpoint
  ✅ Sanitización de strings (strip HTML)
  ✅ Validación RFC con regex estricto
  ⬜ JWE (JWT cifrado) para registrationToken   ← pendiente

Autenticación
  ✅ JWT HS256 con JWT_SECRET desde .env
  ✅ TTL diferenciado (15min registro / 8h sesión)
  ✅ Cookie httpOnly + sameSite=lax

Autorización
  ✅ Middleware authenticate en rutas protegidas
  ✅ Middleware isAdmin para /provisioning/retry
  ✅ apiUrl del tenant aislado en JWT

Rate Limiting
  ✅ 5 req/15min en /initiate
  ✅ 10 req/15min en /verify
  ⬜ Bloqueo de IP por intentos fallidos reiterados  ← pendiente

Secretos
  ✅ Credenciales Velneo solo en .env
  ✅ sessionToken en Redis con TTL
  ⬜ sessionToken cifrado en Redis                   ← recomendado
  ✅ bcrypt 10 rounds para passwords
```
