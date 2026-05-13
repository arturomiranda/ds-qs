# Tabla: ID: roles

## 📄 Información General
- Tipo de tabla: Maestro
- Reside en: Disco
- Longitud del registro: 360
- Número de campos: 4
- Número de índices: 2

## 📝 Descripción [IA]
Esta tabla gestiona los perfiles de acceso y niveles de autorización dentro del portal administrativo del ecosistema DattaErp. Su función principal es segmentar las capacidades operativas de los usuarios internos, permitiendo diferenciar entre administradores totales, supervisores de infraestructura y personal de soporte. Es la base del sistema de seguridad RBAC (Role-Based Access Control) que protege los endpoints sensibles del backend.

## 🛠️ Estructura de Campos

| Identificador | Nombre | Tipo | Longitud | Tipo de enlace | Comentarios y Relaciones |
| :--- | :--- | :--- | :--- | :--- | :--- |
| `id` | ID Rol | Numérico | 10 | | Clave primaria interna. |
| `nombre_rol` | Nombre del Rol | Alfa | 50 | | Identificador único del perfil (ej. ADMIN, USER). Relacionado con el middleware de autorización. |
| `descripcion` | Descripción | Alfa | 255 | | Explicación detallada de los permisos asociados al rol. |
| `es_activo` | Activo | Booleano | 1 | | Control de vigencia del rol. Si es 0, los usuarios vinculados pierden privilegios. |

## 🔍 Índices

| Identificador | Nombre | Tipo de índice |
| :--- | :--- | :--- |
| `PRIMARY` | Primary Key | Clave única (id) |
| `idx_nombre_rol_unico` | Nombre Único | Clave única (nombre_rol) |

## 🔗 Enlaces Plurales
- **usuarios:** Un rol puede estar asignado a múltiples usuarios.

## 📌 Notas [IA]
- El diseño permite una gestión dinámica de permisos sin necesidad de modificar el esquema de base de datos.
- Se recomienda no eliminar registros de esta tabla para preservar la integridad referencial histórica; en su lugar, utilizar el campo `es_activo`.
