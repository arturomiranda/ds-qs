# Tabla: ID: velneo_carpetas

## 📄 Información General
- Tipo de tabla: Maestro
- Reside en: Disco
- Longitud del registro: 210
- Número de campos: 6
- Número de índices: 2

## 📝 Descripción [IA]
Esta tabla gestiona la topología de almacenamiento físico en el servidor Velneo. Permite implementar una estrategia de escalado horizontal manual al organizar a los usuarios en diferentes carpetas (buckets). Controla las cuotas de ocupación para evitar la saturación de un solo directorio, lo cual es fundamental para el rendimiento del vServer y la gestión de backups segmentados.

## 🛠️ Estructura de Campos

| Identificador | Nombre | Tipo | Longitud | Tipo de enlace | Comentarios y Relaciones |
| :--- | :--- | :--- | :--- | :--- | :--- |
| `id` | ID Carpeta | Numérico | 10 | | Clave primaria. |
| `nombre` | Nombre Carpeta | Alfa | 100 | | Nombre físico en el vServer (ej. clientes01). |
| `contador_instancias` | Ocupación | Numérico | 11 | | Cantidad de tenants alojados actualmente. |
| `limite_maximo` | Capacidad | Numérico | 11 | | Máximo de usuarios permitidos en esta carpeta. |
| `estado` | Estado | Enum | - | | disponible, llena, mantenimiento. Controla si se pueden asignar nuevos usuarios. |
| `fecha_creacion` | Registrada el | Tiempo | 4 | | Fecha de alta de la carpeta en el sistema. |

## 🔍 Índices

| Identificador | Nombre | Tipo de índice |
| :--- | :--- | :--- |
| `PRIMARY` | Primary Key | Clave única (id) |
| `idx_nombre_carpeta` | Nombre Único | Clave única (nombre) |

## 🔗 Enlaces Plurales
- **velneo:** Múltiples registros de infraestructura apuntan a una misma carpeta.

## 📌 Notas [IA]
- Es imperativo que el `contador_instancias` se actualice dentro de una transacción al momento de asignar una nueva instancia en la tabla `velneo`.
- Cuando el contador llega al límite, el sistema de aprovisionamiento debe buscar automáticamente la siguiente carpeta con estado `disponible`.
