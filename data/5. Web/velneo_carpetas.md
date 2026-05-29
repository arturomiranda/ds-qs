# Tabla: ID: velneo_carpetas

## 📄 Información General
- Tipo de tabla: Maestro
- Reside en: Disco
- Longitud del registro: 300
- Número de campos: 7
- Número de índices: 2

## 📝 Descripción [IA]
Esta tabla es el orquestador físico de la infraestructura en la nube. Gestiona los contenedores (carpetas) de Velneo Cloud donde se alojan las instancias de los clientes. Su propósito es permitir un escalamiento organizado: cuando una carpeta alcanza su límite máximo de instancias, el sistema de aprovisionamiento utiliza esta tabla para identificar la siguiente carpeta disponible, evitando la saturación de recursos y facilitando las tareas de mantenimiento por bloques de clientes.

## 🛠️ Estructura de Campos

| Identificador | Nombre | Tipo | Longitud | Tipo de enlace | Comentarios y Relaciones |
| :--- | :--- | :--- | :--- | :--- | :--- |
| `id` | ID Carpeta | Numérico | 10 | | Clave primaria e identificador interno. |
| `nombre` | Nombre Físico | Alfa | 100 | | Nombre de la carpeta en el vServer (ej: 'clientes_zona_a'). |
| `contador_instancias` | Ocupación | Numérico | 11 | | Cantidad actual de tenants alojados en esta carpeta. |
| `limite_maximo` | Capacidad | Numérico | 11 | | Límite de instancias permitidas antes de marcar como llena. |
| `estado` | Estatus Ops. | Enum | | | 'disponible', 'llena' o 'mantenimiento'. |
| `fecha_creacion` | Alta | Tiempo | 4 | | Registro de creación de la carpeta. |
| `fecha_actualizacion` | Último Cambio | Tiempo | 4 | | Seguimiento de cambios en capacidad o estado. |

## 🔍 Índices

| Identificador | Nombre | Tipo de índice | Campos |
| :--- | :--- | :--- | :--- |
| `PRIMARY` | Primary Key | Clave única | `id` |
| `idx_nombre_carpeta` | Nombre Único | Clave única | `nombre` |

## 🔗 Enlaces Plurales
- **velneo:** Una carpeta agrupa múltiples configuraciones de tenants.

## 📌 Notas [IA]
- El sistema de registro automático consulta esta tabla buscando la primera carpeta con `estado = 'disponible'` y `contador_instancias < limite_maximo`.
- Es vital mantener el campo `nombre` sincronizado con la estructura real de carpetas en el Administrador de Velneo Cloud.