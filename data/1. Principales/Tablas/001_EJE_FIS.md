# Tabla: ID: 001_EJE_FIS
# Nombre: 001: Ejercicios Fiscales

## 📄 Información General
- Tipo de tabla: Maestro
- Reside en: Disco
- Longitud del registro: 15
- Número de campos: 5
- Número de índices: 6

## 📝 Descripción
Esta tabla gestiona los periodos anuales de operación contable y fiscal del ERP. Cada registro representa un año fiscal específico para una Empresa o Sucursal, permitiendo el aislamiento temporal de la información financiera. Es el nivel superior de la jerarquía temporal, del cual dependen los Periodos mensuales (`001_PERI`), y sirve como filtro global para la generación de balances, estados de resultados y cierres de ejercicio.

## 🛠️ Estructura de Campos

| Identificador | Nombre | Tipo | Longitud | Tipo de enlace | Comentarios y Relaciones |
| :--- | :--- | :--- | :--- | :--- | :--- |
| `ID` | Código | Numérico | 3 | | Identificador único del ejercicio. |
| `EMP` | Empresa | Numérico | 3 | Maestro: `001_EMP@datta_erp_dat` | Empresa a la que pertenece el ejercicio fiscal. |
| `SUC` | Sucursal | Numérico | 3 | Maestro: `001_EMP@datta_erp_dat` | Sucursal específica vinculada al ejercicio. |
| `ANIO` | Año | Numérico | 4 | | El año fiscal representado (ej. 2026). |
| `EST_EJE` | Estatus del ejercicio | Alfa 256 | 1 | | Estado actual (A: Abierto, C: Cerrado, P: Preparación). |

## 🔍 Índices

| Identificador | Nombre | Tipo de índice |
| :--- | :--- | :--- |
| `ID` | Código | Clave única |
| `EMP_ANI` | Empresa y Año | Clave única |
| `ANI_ORD` | Orden por Año | Acepta repetidas |
| `EST_EJE_ACT` | Estatus del ejercicio | Acepta repetidas |
| `EMP` | Empresa | Acepta repetidas |
| `SUC` | Sucursal | Acepta repetidas |

## 🔗 Enlaces Plurales

| Identificador | Nombre | Tabla enlazada | Índice |
| :--- | :--- | :--- | :--- |
| `001_PERI_EJER_MES` | 001: Periodos | `001_PERI@datta_erp_dat` | `EJER_MES` |

## ⚡ Triggers
*No reporta triggers definidos.*

## 📌 Notas
La unicidad del índice `EMP_ANI` garantiza que no existan dos registros para el mismo año en la misma empresa. El control de estatus (`EST_EJE`) debe ser respetado por los procesos de contabilidad para evitar inserciones en años ya cerrados fiscalmente. Se recomienda que la apertura de un nuevo ejercicio dispare automáticamente la creación de sus 12 periodos mensuales correspondientes en la tabla `001_PERI`.
