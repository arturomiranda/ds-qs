# Tabla: ID: usuarios

## 📄 Información General
- Tipo de tabla: Maestro
- Reside en: Disco
- Longitud del registro: 1250
- Número de campos: 16
- Número de índices: 4

## 📝 Descripción [IA]
Repositorio central de identidades para el sistema SaaS. Esta tabla almacena la información de perfil, contacto y seguridad de todos los suscriptores y administradores. Es la entidad "Ancla" del sistema: cada registro aquí representa un suscriptor potencial o activo que requiere recursos de infraestructura en Velneo Cloud. Gestiona el ciclo de vida inicial desde el registro (Landing) hasta la verificación por OTP.

## 🛠️ Estructura de Campos

| Identificador | Nombre | Tipo | Longitud | Tipo de enlace | Comentarios y Relaciones |
| :--- | :--- | :--- | :--- | :--- | :--- |
| `id` | ID Usuario | Numérico | 10 | | Clave primaria. ID autoincremental. |
| `nombres` | Nombre(s) | Alfa | 100 | | Nombre de pila del usuario. |
| `apellido_paterno` | Apellido Paterno | Alfa | 100 | | Primer apellido. |
| `apellido_materno` | Apellido Materno | Alfa | 100 | | Segundo apellido. |
| `empresa` | Empresa | Alfa | 100 | | Razón social o nombre comercial del suscriptor. |
| `rfc` | RFC | Alfa | 13 | | Identificador fiscal (México). Crucial para validación de identidad. |
| `correo` | Email | Alfa | 100 | | Login principal y canal de comunicación para OTP. |
| `telefono` | Teléfono | Alfa | 20 | | Contacto telefónico opcional. |
| `usuario` | Username | Alfa | 50 | | Alias de acceso al sistema (ej. juan.perez). |
| `contraseña` | Password | Alfa | 255 | | Hash de seguridad de la contraseña (Bcrypt). |
| `id_rol` | Rol | Numérico | 10 | Maestro: `roles` | Vínculo con los permisos del usuario. |
| `fecha_registro` | Creado el | Tiempo | 4 | | Timestamp automático de creación. |
| `verificado` | Verificado | Booleano | 1 | | Indica si el usuario superó la validación OTP. |
| `codigo_otp` | Código OTP | Alfa | 6 | | Token temporal de 6 dígitos para validación. |
| `fecha_expiracion_otp` | Expira OTP | Tiempo | 4 | | Límite temporal para usar el código OTP. |
| `actualizar_contraseña` | Forzar Cambio | Booleano | 1 | | Obliga al usuario a cambiar su contraseña en el primer login. |

## 🔍 Índices

| Identificador | Nombre | Tipo de índice |
| :--- | :--- | :--- |
| `PRIMARY` | Primary Key | Clave única (id) |
| `idx_correo_unico` | Correo Único | Clave única (correo) |
| `idx_usuario_unico` | Usuario Único | Clave única (usuario) |
| `FK_usuarios_roles` | Relación Rol | Acepta repetidas (id_rol) |

## 🔗 Enlaces Plurales
- **velneo:** Un usuario posee una configuración de infraestructura vinculada.

## 📌 Notas [IA]
- La tabla está optimizada para búsquedas rápidas por `correo` o `usuario` para agilizar el proceso de login.
- El campo `contraseña` NUNCA debe almacenarse en texto plano.
- La integridad con la tabla `roles` es obligatoria (`ON DELETE RESTRICT`).
