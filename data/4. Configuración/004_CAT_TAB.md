# Tabla: 004: Catalogo de Tablas

Esta tabla maestra centraliza el catálogo de todas las tablas físicas y lógicas del sistema. Es utilizada principalmente para la gestión dinámica de permisos y auditoría.

## 📄 Información General
- **ID de Tabla:** `004_CAT_TAB`
- **Tipo:** Maestro
- **Reside en:** Disco
- **Longitud del registro:** 516
- **Número de campos:** 3
- **Número de índices:** 5
- **Descripción:** Catálogo de definición de tablas para el motor de permisos y sistema.

## 🛠️ Estructura de Campos

| Identificador | Nombre | Tipo | Longitud | Tipo de enlace |
| :--- | :--- | :--- | :--- | :--- |
| `ID` | Código | Numérico | 3 | |
| `NAME` | Name | Alfa 256 | 256 | |
| `IDE` | Identificador | Alfa Latin1 | 256 | |

## 🔍 Índices

| Identificador | Nombre | Tipo de indice |
| :--- | :--- | :--- |
| `ID` | Código | Clave única |
| `NAME` | Alfabético | Acepta repetidas |
| `WORDS` | Palabras | Palabras |
| `PARTS` | Trozos de palabras | Trozos de palabras |
| `IDE` | Identificador | Acepta repetidas |

## 📝 Notas
- El campo `IDE` almacena el identificador técnico de la tabla en Velneo (ej. `001_ENT`).
- Es una tabla crítica para el módulo de Permisos (`001_PRM`), ya que permite asignar privilegios por tabla.
