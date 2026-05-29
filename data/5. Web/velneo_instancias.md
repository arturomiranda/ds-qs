# Tabla: ID: velneo_instancias

## 📄 Información General
- Tipo de tabla: Maestro
- Reside en: Disco
- Longitud del registro: 1200
- Número de campos: 11
- Número de índices: 2

## 📝 Descripción [IA]
Actúa como el inventario técnico de los activos desplegados en Velneo Cloud. Almacena la metadata específica de cada instancia (DAT para datos, APP para aplicaciones) creada para los clientes. Esta tabla es la fuente de verdad que utiliza el Backend para saber qué proyecto VCD y qué ruta de archivos debe utilizar al momento de conectar a un vServer específico. Es el puente entre el mundo relacional de MySQL y el mundo de objetos de Velneo.

## 🛠️ Estructura de Campos

| Identificador | Nombre | Tipo | Longitud | Tipo de enlace | Comentarios y Relaciones |
| :--- | :--- | :--- | :--- | :--- | :--- |
| `id` | ID Registro | Numérico | 10 | | Clave primaria. |
| `id_instancia` | ID vServer | Alfa | 100 | | Identificador único devuelto por la API de Velneo (ej: 'PROD_DAT'). |
| `nombre` | Nombre Alias | Alfa | 100 | | Nombre amigable de la instancia. |
| `ruta` | Ruta Cloud | Alfa | 255 | | Ubicación física de los datos en el sistema de archivos del cloud. |
| `proyecto` | Archivo VCD | Alfa | 100 | | Nombre del proyecto base asociado (ej: 'erp_core.vcd'). |
| `solucion` | Solución | Alfa | 100 | | Nombre de la solución Velneo que contiene la instancia. |
| `codigo` | Código Serv. | Numérico | 11 | | Código interno de respuesta del servidor de administración. |
| `tipo` | Tipo Inst. | Enum | | | Clasifica si es una instancia de datos ('dat') o de aplicación ('app'). |
| `fecha_creacion` | Alta | Tiempo | 4 | | Registro del despliegue inicial. |
| `fecha_actualizacion` | Sincronización | Tiempo | 4 | | Última vez que se refrescaron los metadatos desde el cloud. |

## 🔍 Índices

| Identificador | Nombre | Tipo de índice | Campos |
| :--- | :--- | :--- | :--- |
| `PRIMARY` | Primary Key | Clave única | `id` |
| `uk_id_instancia_tipo` | ID Único | Clave única | `id_instancia`, `tipo` |

## 🔗 Enlaces Plurales
- **velneo:** Una instancia es referenciada por las configuraciones de los tenants.

## 📌 Notas [IA]
- La combinación de `id_instancia` y `tipo` debe ser siempre única para evitar ambigüedad en el enrutamiento.
- Esta tabla debe actualizarse automáticamente cada vez que se realice un cambio de arquitectura o actualización de versiones en los proyectos de Velneo.