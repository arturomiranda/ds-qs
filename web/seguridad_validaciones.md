# 🔐 Seguridad, Validación y Cumplimiento

Este documento establece las políticas de seguridad y las capas de validación que deben implementarse en el ecosistema **Datta-Erp**.

## 🛡️ Capas de Seguridad

### 1. Autenticación (JWT)
- **Token**: Se utilizará `jsonwebtoken` para emitir Access Tokens.
- **Expiración**: 1 hora para Access Tokens.
- **Almacenamiento**: Cookies con flags `Secure`, `SameSite=Strict` y `HttpOnly` (para evitar XSS).

### 2. Protección de API (Backend)
- **Helmet**: Para asegurar headers HTTP contra vulnerabilidades comunes.
- **Express Rate Limit**: Máximo 100 peticiones por cada 15 minutos por IP para prevenir fuerza bruta.
- **CORS**: Configurado estrictamente para permitir solo el dominio del Frontend.

### 3. Validación de Datos
- **Backend**: Uso de middlewares de validación (ej. `Joi` o `Zod`) antes de procesar cualquier `req.body`.
- **Frontend**: `React-Hook-Form` para validación inmediata en el cliente (campos obligatorios, formatos de RFC, correos).

---

## 🏗️ Multi-tenancy (Aislamiento de Datos)

El sistema garantiza que los datos de un cliente no sean visibles para otros mediante:

1. **Aislamiento en MySQL**: Cada registro en la tabla `velneo` está vinculado estrictamente a un `id_usuario`.
2. **Aislamiento en Velneo**: Cada cliente tiene sus propias **instancias DAT y APP**, eliminando el riesgo de fugas de datos a nivel de base de datos.
3. **Middleware de Propiedad**: En el backend, cada petición a `/api/erp/*` verifica que la `url_api` que se intenta consumir pertenece efectivamente al usuario autenticado en el JWT.

---

## 📜 Estándares de Código y Calidad

### Backend (Node/Express)
- Uso de `morgan` para registro de auditoría de todas las peticiones de escritura.
- Manejo centralizado de errores mediante un middleware global.
- Encriptación de contraseñas con `bcrypt` (salting rounds: 10).

### Frontend (Next.js)
- Tipado estricto con **TypeScript**.
- Uso de `TanStack Query` para evitar peticiones redundantes y manejar estados de carga/error globalmente.
- Separación de componentes presentacionales y contenedores lógicos.

---

## 🚦 Checklist de Lanzamiento
- [ ] Configurar variables de entorno (`.env`) seguras.
- [ ] Implementar certificados SSL (HTTPS).
- [ ] Realizar pruebas de carga en la API de Velneo.
- [ ] Verificar que el OTP expira correctamente después de 10 minutos.
