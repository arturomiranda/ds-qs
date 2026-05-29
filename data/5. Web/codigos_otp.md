# Tabla: ID: codigos_otp

## 📄 Información General
- Tipo de tabla: Maestro
- Reside en: Disco
- Longitud del registro: 250
- Número de campos: 7
- Número de índices: 3

## 📝 Descripción [IA]
Esta tabla centraliza la gestión de seguridad temporal para procesos críticos como el registro de nuevos usuarios, la recuperación de contraseñas y la autenticación de dos factores (2FA). Al estar desacoplada de la tabla de usuarios, permite generar códigos para prospectos que aún no han completado su registro, manteniendo la integridad del Hub principal. Es el motor de confianza que asegura que el correo proporcionado es controlado por el solicitante.

## 🛠️ Estructura de Campos

| Identificador | Nombre | Tipo | Longitud | Tipo de enlace | Comentarios y Relaciones |
| :--- | :--- | :--- | :--- | :--- | :--- |
| `id` | ID Registro | Numérico | 10 | | Clave primaria autoincremental. |
| `correo` | Correo Destino | Alfa | 100 | | Dirección a la que se envió el código (enlace lógico con `usuarios.correo`). |
| `codigo` | Token OTP | Alfa | 6 | | Código numérico de 6 dígitos autogenerado. |
| `tipo` | Contexto | Enum | | | Define el propósito: 'registro', 'reset_contrasena' o '2fa'. |
| `fecha_expiracion` | Expira el | Tiempo | 4 | | Límite de validez del código (configurado por defecto a 15 min). |
| `usado` | Estado Uso | Booleano | 1 | | Bandera de seguridad para evitar ataques de reutilización (1 = Ya usado). |
| `fecha_creacion` | Emitido el | Tiempo | 4 | | Registro del momento exacto de generación. |

## 🔍 Índices

| Identificador | Nombre | Tipo de índice | Campos |
| :--- | :--- | :--- | :--- |
| `PRIMARY` | Primary Key | Clave única | `id` |
| `idx_otp_correo_tipo` | Búsqueda Rápida | Acepta repetidas | `correo`, `tipo` |
| `idx_otp_expiracion` | Control Tiempo | Acepta repetidas | `fecha_expiracion` |

## 🔗 Enlaces Plurales
- *No reporta enlaces plurales salientes.*

## 📌 Notas [IA]
- El índice `idx_otp_expiracion` es vital para procesos de limpieza (garbage collection) de códigos antiguos.
- Un código se considera válido SOLO SI `usado = 0` Y `fecha_expiracion > NOW()`.
- Se recomienda una tarea programada (cron) para eliminar registros de más de 24 horas para mantener el rendimiento.