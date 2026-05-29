# Tabla: ID: sesiones

## 📄 Información General
- Tipo de tabla: Maestro
- Reside en: Disco
- Longitud del registro: 1000
- Número de campos: 7
- Número de índices: 4

## 📝 Descripción [IA]
Actúa como la capa de control de acceso dinámico y auditoría. Registra cada sesión activa emitida por el servidor de autenticación, permitiendo un seguimiento en tiempo real de quién está conectado y desde dónde. Su función técnica es permitir la invalidación selectiva de tokens JWT (Blacklisting/Revocation) antes de que expiren naturalmente, proporcionando una capa extra de seguridad en caso de robo de credenciales o cierre de sesión global.

## 🛠️ Estructura de Campos

| Identificador | Nombre | Tipo | Longitud | Tipo de enlace | Comentarios y Relaciones |
| :--- | :--- | :--- | :--- | :--- | :--- |
| `id` | ID Sesión | Numérico | 10 | | Clave primaria. |
| `id_usuario` | Dueño | Numérico | 10 | Maestro: `usuarios` | Referencia al usuario que inició la sesión. |
| `hash_token` | Huella JWT | Alfa | 255 | | Firma SHA-256 del token para validación sin almacenar el secreto original. |
| `fecha_expiracion` | Límite Sesión | Tiempo | 4 | | Momento en que la sesión deja de ser válida legalmente. |
| `direccion_ip` | IP Origen | Alfa | 45 | | Dirección IPv4 o IPv6 para rastreo de seguridad. |
| `agente_usuario` | Dispositivo | Alfa | 512 | | Información del navegador/cliente (User-Agent). |
| `fecha_creacion` | Inicio Sesión | Tiempo | 4 | | Timestamp del login exitoso. |

## 🔍 Índices

| Identificador | Nombre | Tipo de índice | Campos |
| :--- | :--- | :--- | :--- |
| `PRIMARY` | Primary Key | Clave única | `id` |
| `idx_hash_token` | Token Único | Clave única | `hash_token` |
| `idx_sesiones_usuario` | Usuario | Acepta repetidas | `id_usuario` |
| `idx_sesiones_expiracion` | Expiración | Acepta repetidas | `fecha_expiracion` |

## 🔗 Enlaces Plurales
- *No reporta enlaces plurales salientes.*

## 📌 Notas [IA]
- El middleware de autorización consulta esta tabla en cada petición REST para confirmar que el `hash_token` sigue activo.
- Al eliminar un registro de esta tabla, el usuario es desconectado automáticamente en su próxima interacción.
- Se implementa `ON DELETE CASCADE` con usuarios para limpiar sesiones automáticamente al borrar una cuenta.