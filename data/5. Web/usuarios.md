# Tabla: ID: usuarios

## 📄 Información General
- Tipo de tabla: Maestro
- Reside en: Disco
- Longitud del registro: 1100
- Número de campos: 15
- Número de índices: 4

## 📝 Descripción [IA]
Entidad "Ancla" del Hub de aplicaciones. Almacena la identidad digital de todos los suscriptores y administradores del sistema Datta-Erp. Su rol es centralizar el acceso y vincular a cada persona con su infraestructura aislada en Velneo Cloud. A diferencia de versiones anteriores, la lógica de validación temporal (OTP) ha sido externalizada para permitir un seguimiento de auditoría más granular y seguro, dejando a esta tabla enfocada exclusivamente en el perfil y estado del usuario.

## 🛠️ Estructura de Campos

| Identificador | Nombre | Tipo | Longitud | Tipo de enlace | Comentarios y Relaciones |
| :--- | :--- | :--- | :--- | :--- | :--- |
| `id` | ID Usuario | Numérico | 10 | | Clave primaria autoincremental. |
| `nombres` | Nombre(s) | Alfa | 100 | | Nombre de pila o nombres legales del usuario. |
| `apellido_paterno` | Apellido P. | Alfa | 100 | | Primer apellido para identificación oficial. |
| `apellido_materno` | Apellido M. | Alfa | 100 | | Segundo apellido. |
| `empresa` | Razón Social | Alfa | 100 | | Nombre de la organización contratante. |
| `rfc` | RFC | Alfa | 13 | | Registro Federal de Contribuyentes (México). Clave para integración SAT. |
| `correo` | Email | Alfa | 100 | | Identificador único de acceso y destino para OTP. |
| `telefono` | Teléfono | Alfa | 20 | | Medio de contacto secundario. |
| `usuario` | Username | Alfa | 50 | | Alias de inicio de sesión (normalmente nombre.apellido). |
| `contraseña` | Password Hash | Alfa | 255 | | Credencial encriptada mediante algoritmo Bcrypt. |
| `id_rol` | Rol ID | Numérico | 10 | Maestro: `roles` | Vinculación con los permisos de acceso del sistema. |
| `verificado` | Estado Verif. | Booleano | 1 | | Indica si el usuario ha confirmado su identidad vía OTP (1 = Sí). |
| `actualizar_contraseña` | Reset Pass | Booleano | 1 | | Bandera para forzar el cambio de contraseña en el próximo acceso. |
| `fecha_creacion` | Registro | Tiempo | 4 | | Fecha y hora en que el usuario se dio de alta. |
| `fecha_actualizacion` | Último Cambio | Tiempo | 4 | | Auditoría de la última modificación en los datos del perfil. |

## 🔍 Índices

| Identificador | Nombre | Tipo de índice | Campos |
| :--- | :--- | :--- | :--- |
| `PRIMARY` | Primary Key | Clave única | `id` |
| `idx_correo_unico` | Correo Único | Clave única | `correo` |
| `idx_usuario_unico` | Usuario Único | Clave única | `usuario` |
| `FK_usuarios_roles` | Relación Rol | Acepta repetidas | `id_rol` |

## 🔗 Enlaces Plurales
- **sesiones:** Un usuario puede tener múltiples sesiones activas (aunque el sistema puede limitarlas).
- **velneo:** Vínculo directo con la configuración de aprovisionamiento del tenant.
- **codigos_otp:** Relación lógica por correo para procesos de validación.

## 📌 Notas [IA]
- El campo `verificado` debe ser consultado por el middleware de autenticación antes de emitir cualquier JWT.
- La tabla está optimizada para búsquedas rápidas por `correo` y `usuario` para minimizar la latencia en el login.
- Se recomienda mantener la integridad referencial con `roles` para evitar usuarios huérfanos.