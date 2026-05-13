# FASE 1 — Contrato de API
> Módulo: **Registro de Instancias Velneo + Usuario**
> Arquitectura: Limpia multi-tenant | Stack: Node/Express + Next.js + MySQL + Redis + BullMQ

---

## Dominio 1: `Registration` — Registro e identidad del usuario
## Dominio 2: `Provisioning` — Aprovisionamiento de infraestructura Velneo

---

## Endpoint 1 — Iniciar Registro

```
POST /api/v1/registration/initiate
```

**Headers:** `Content-Type: application/json`
**Auth:** Ninguna

**Request Body:**
```json
{
  "firstName":  "string | requerido | min:2 max:50",
  "lastNameP":  "string | requerido | min:2 max:50",
  "lastNameM":  "string | opcional  | max:50",
  "email":      "string | requerido | formato email",
  "phone":      "string | opcional  | formato E.164",
  "company":    "string | opcional  | max:100",
  "rfc":        "string | opcional  | regex:[A-ZÑ&]{3,4}[0-9]{6}[A-Z0-9]{3}"
}
```

**Response 202 Accepted:**
```json
{
  "success": true,
  "message": "Código de verificación enviado al correo",
  "data": {
    "registrationToken": "<JWT firmado | TTL 15min>",
    "email":             "usuario@empresa.com",
    "expiresAt":         "2026-05-13T11:45:00Z"
  }
}
```

**Errores:**
| Código | Causa |
|---|---|
| `400` | Email ya registrado y verificado (local o Cloud) |
| `422` | Body con campos inválidos (validación DTO) |
| `503` | Velneo Cloud API no disponible |

---

## Endpoint 2 — Verificar OTP

```
POST /api/v1/registration/verify
```

**Headers:** `Content-Type: application/json`
**Auth:** Ninguna

**Request Body:**
```json
{
  "email":             "string | requerido",
  "code":              "string | requerido | 6 dígitos numéricos",
  "registrationToken": "string | requerido | JWT"
}
```

**Response 200 OK:**
```json
{
  "success": true,
  "message": "Cuenta creada. Aprovisionamiento iniciado.",
  "data": {
    "userId":         42,
    "provisioningId": "PROV-abc123",
    "status":         "provisioning_started",
    "statusUrl":      "/api/v1/provisioning/status/PROV-abc123"
  }
}
```

> 🔵 **Decisión arquitectónica:** La respuesta ya **no espera** a que el aprovisionamiento termine.
> Devuelve un `provisioningId` y una URL de estado para polling.

**Errores:**
| Código | Causa |
|---|---|
| `400` | OTP incorrecto o token expirado |
| `409` | Usuario ya verificado (reintento duplicado) |

---

## Endpoint 3 — Estado del Aprovisionamiento *(nuevo)*

```
GET /api/v1/provisioning/status/:provisioningId
```

**Headers:** `Authorization: Bearer <JWT>`
**Auth:** Requerida

**Response 200 OK:**
```json
{
  "success": true,
  "data": {
    "provisioningId": "PROV-abc123",
    "status":         "completed | in_progress | failed",
    "checkpoints": {
      "folder":       "completed",
      "dataInstance": "completed",
      "appInstance":  "completed",
      "group":        "completed",
      "user":         "completed"
    },
    "velneo": {
      "folder":         "cloud01",
      "username":       "juan.perez123",
      "appInstanceId":  42,
      "dataInstanceId": "INS_DAT_001"
    },
    "completedAt": "2026-05-13T11:42:00Z"
  }
}
```

---

## Endpoint 4 — Reintentar Aprovisionamiento fallido *(nuevo)*

```
POST /api/v1/provisioning/retry/:provisioningId
```

**Headers:** `Authorization: Bearer <JWT admin>`
**Auth:** Rol Admin

**Response 202 Accepted:**
```json
{
  "success": true,
  "message": "Reintento de aprovisionamiento iniciado",
  "data": { "provisioningId": "PROV-abc123", "status": "retrying" }
}
```

---

## Trade-off — Aprovisionamiento Síncrono vs Asíncrono

| | **Opción A: Síncrono (estado anterior)** | **Opción B: Asíncrono (adoptado)** |
|---|---|---|
| ✅ Pros | Implementación simple | Sin timeouts HTTP, resiliente a fallos de red |
| ✅ Pros | El cliente recibe el resultado final en una sola llamada | Permite reintentos automáticos por checkpoint |
| 🔴 Contras | Timeout si Velneo Cloud tarda +30s | Requiere polling o WebSocket en el frontend |
| 🔴 Contras | Sin recuperación si el request falla a mitad | Mayor complejidad inicial |

> **Decisión adoptada:** Opción B con BullMQ para los jobs de aprovisionamiento + sesión Velneo cacheada en Redis.

---

## Decisión: Validación de Email en Velneo Cloud

**Estrategia adoptada: Validación Síncrona con Sesión Cacheada**

- Un proceso Cron en background mantiene un `sessionToken` de Velneo vivo en Redis (TTL: 29 min, renovación cada 25 min).
- El endpoint `/initiate` solo ejecuta el paso de validación (`checkUserExists`) usando el token del cache.
- Tiempo de validación: **~milisegundos** vs ~2-5 segundos sin cache.
- Si Redis falla: **fallback** → crear sesión directa de emergencia.
