# Tabla: ID: roles

## 📄 Información General
- Tipo de tabla: Maestro
- Reside en: Disco
- Longitud del registro: 360
- Número de campos: 6
- Número de índices: 2

## 📝 Descripción [IA]
Esta tabla actúa como el catálogo maestro de perfiles de acceso dentro del ecosistema Datta-Erp. Su propósito fundamental es segmentar las capacidades operativas de los usuarios, permitiendo que el sistema asigne permisos específicos según la función de negocio (ej. Administrador Global vs. Usuario de Tenant). Es una pieza clave para la seguridad basada en roles (RBAC) que consume el Backend durante la validación del JWT.

## 🛠️ Estructura de Campos

| Identificador | Nombre | Tipo | Longitud | Tipo de enlace | Comentarios y Relaciones |
| :--- | :--- | :--- | :--- | :--- | :--- |
| `id` | ID Rol | Numérico | 10 | | Clave primaria. Identificador único para la vinculación con usuarios. |
| `nombre_rol` | Nombre | Alfa | 50 | | Etiqueta única del rol (ej: 'admin', 'cliente'). |
| `descripcion` | Descripción | Alfa | 255 | | Detalle de las responsabilidades o permisos que otorga el rol. |
| `es_activo` | Activo | Booleano | 1 | | Control lógico de habilitación. Si es 0, los usuarios con este rol no pueden loguearse. |
| `fecha_creacion` | Creado el | Tiempo | 4 | | Timestamp de auditoría para el registro inicial. |
| `fecha_actualizacion` | Actualizado el | Tiempo | 4 | | Registro automático de la última modificación del rol. |

## 🔍 Índices

| Identificador | Nombre | Tipo de índice | Campos |
| :--- | :--- | :--- | :--- |
| `PRIMARY` | Primary Key | Clave única | `id` |
| `idx_nombre_rol_unico` | Rol Único | Clave única | `nombre_rol` |

## 🔗 Enlaces Plurales
- **usuarios:** Un rol es compartido por múltiples usuarios de la plataforma.

## 📌 Notas [IA]
- La integridad de esta tabla es crítica. Se recomienda no eliminar roles que tengan usuarios asociados (`ON DELETE RESTRICT`).
- Los nombres de roles deben ser normalizados (preferiblemente minúsculas) para evitar colisiones en la lógica del middleware de autorización.