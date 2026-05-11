# Tabla: ID: 004_CAT_TAB

## 📄 Información General
- Tipo de tabla: Maestro
- Reside en: Disco
- Longitud del registro: 516
- Número de campos: 3
- Número de índices: 5

## 📝 Descripción
Esta tabla funciona como el Diccionario de Datos dinámico del ERP. Almacena la lista de todas las tablas que componen la base de datos del proyecto. Su función primordial es servir de catálogo de referencia para el motor de permisos granulares (`001_PRM`), permitiendo que el administrador del sistema pueda asignar privilegios sobre tablas específicas sin necesidad de modificar el código fuente.

## 🛠️ Estructura de Campos

| Identificador | Nombre | Tipo | Longitud | Tipo de enlace | Comentarios y Relaciones |
| :--- | :--- | :--- | :--- | :--- | :--- |
| `ID` | Código | Numérico | 3 | | ID único correlativo del catálogo. |
| `NAME` | Name | Alfa 256 | 256 | | Nombre descriptivo de la tabla (ej. "Entidades"). |
| `IDE` | Identificador | Alfa Latin1 | 256 | | Identificador técnico de la tabla en Velneo (ej. "001_ENT"). |

## 🔍 Índices

| Identificador | Nombre | Tipo de índice |
| :--- | :--- | :--- |
| `ID` | Código | Clave única |
| `NAME` | Alfabético | Acepta repetidas |
| `WORDS` | Palabras | Palabras |
| `PARTS` | Trozos de palabras | Trozos de palabras |
| `IDE` | Identificador | Acepta repetidas |

## 🔗 Enlaces Plurales
*No reporta enlaces plurales.*

## ⚡ Triggers
*No definidos.*

## 📌 Notas
El campo `IDE` debe coincidir exactamente con el nombre del objeto tabla en el editor de Velneo para que los procesos de seguridad funcionen correctamente. Esta tabla debe actualizarse cada vez que se agregue un nuevo módulo o tabla al esquema físico de la base de datos. Se recomienda un proceso de sincronización automática que lea el esquema y actualice este catálogo.
