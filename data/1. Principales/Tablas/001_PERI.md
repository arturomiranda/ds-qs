# Tabla: ID: 001_PERI
# Nombre: 001: Periodos


## 📄 Información General
- Tipo de tabla: Maestro
- Reside en: Disco
- Longitud del registro: 30
- Número de campos: 10
- Número de índices: 5

## 📝 Descripción
Esta tabla define la segmentación mensual de los ejercicios fiscales. Su función principal es controlar el flujo operativo del ERP mediante periodos abiertos o cerrados. Cada transacción contable o financiera debe validar si el periodo correspondiente está habilitado para escritura. Al incluir ahora enlaces a `EMP` y `SUC`, permite cierres mensuales independientes por cada unidad de negocio.

## 🛠️ Estructura de Campos

| Identificador | Nombre | Tipo | Longitud | Tipo de enlace | Comentarios y Relaciones |
| :--- | :--- | :--- | :--- | :--- | :--- |
| `ID` | Código | Numérico | 3 | | ID único del periodo. |
| `EMP` | Empresa | Numérico | 3 | Maestro: `001_EMP@datta_erp_dat` | Empresa a la que pertenece el periodo. |
| `SUC` | Sucursal | Numérico | 3 | Maestro: `001_EMP@datta_erp_dat` | Sucursal específica vinculada al periodo. |
| `EJER` | Ejercicio | Numérico | 3 | Maestro: `001_EJE_FIS@datta_erp_dat` | Enlace al ejercicio fiscal anual padre. |
| `MES_PER` | Mes | Numérico | 2 | | Número del mes (1-12). |
| `EST_PER` | Estatus del Periodo | Alfa 256 | 1 | | Control de acceso (A: Abierto, C: Cerrado). |
| `CRD_USR` | Creado por | Numérico | 3 | Maestro: `001_USU@datta_erp_dat` | Auditoría de creación. |
| `CRD_TIM` | Creado el | Tiempo | 4 | | Timestamp de creación. |
| `MDF_USR` | Modificado por | Numérico | 3 | Maestro: `001_USU@datta_erp_dat` | Auditoría de última edición. |
| `MDF_TIM` | Modificado el | Tiempo | 4 | | Timestamp de modificación. |

## 🔍 Índices

| Identificador | Nombre | Tipo de índice |
| :--- | :--- | :--- |
| `ID` | Código | Clave única |
| `EJER_MES` | Mes por Ejercicio | Clave única |
| `EST_PER` | Estatus del Periodo | Acepta repetidas |
| `EMP` | Empresa | Acepta repetidas |
| `SUC` | Sucursal | Acepta repetidas |

## 🔗 Enlaces Plurales

| Identificador | Nombre | Tabla enlazada | Índice |
| :--- | :--- | :--- | :--- |
| `002_POL_CTRL_FOL` | 002: Pólizas | `002_POL@datta_erp_dat` | `CTRL_FOL` |

## ⚡ Triggers
- **Alta:** Anterior a un alta de ficha
- **Modificación:** Anterior a una modificación de ficha

## 📌 Notas
Se ha incrementado la longitud de registro para dar soporte a la segmentación Multi-Tenant (`EMP`/`SUC`) a nivel de mes. El índice único `EJER_MES` garantiza que no se traslapen periodos dentro del mismo ejercicio contable. Es una tabla de alta consulta para validaciones de integridad transaccional.
