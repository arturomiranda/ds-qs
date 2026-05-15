# Tabla: ID: 002_CTA_CNT
# Nombre: 002: Cuentas Contables

## 📄 Información General
- Tipo de tabla: Maestro
- Reside en: Disco
- Longitud del registro: 288
- Número de campos: 8
- Número de índices: 6

## 📝 Descripción
El Catálogo Maestro de Cuentas Contables. Es el eje estructural de toda la información financiera del ERP. Organiza de forma jerárquica los activos, pasivos, capital, ingresos y egresos. Incluye compatibilidad nativa con la Contabilidad Electrónica del SAT mediante el enlace con el código agrupador oficial. Define comportamientos críticos como la obligatoriedad de centros de costo o el nivel de la cuenta.

## 🛠️ Estructura de Campos

| Identificador | Nombre | Tipo | Longitud | Tipo de enlace | Comentarios y Relaciones |
| :--- | :--- | :--- | :--- | :--- | :--- |
| `ID` | Código | Numérico | 3 | | Identificador interno correlativo. |
| `NAME` | Nombre de la cuenta | Alfa 256 | 256 | | Nombre oficial de la cuenta contable. |
| `PAD_CTA` | Cuenta Padre | Numérico | 3 | Maestro: `002_CTA_CNT@datta_erp_dat` | Define la jerarquía (Cuentas de Mayor y Subcuentas). |
| `CTA_SAT` | Cuenta SAT | Alfa 256 | 20 | | Código agrupador del SAT para cumplimiento fiscal. |
| `NAT_CTA` | Naturaleza | Alfa 256 | 1 | | Define comportamiento: D (Deudora) o A (Acreedora). |
| `NIV_CTA` | Nivel | Numérico | 2 | | Nivel jerárquico dentro del árbol de cuentas (1, 2, 3...). |
| `REG_CCO` | Registra CCO | Booleano | 1 | | Si es Verdadero, obliga a capturar Centro de Costo en asientos. |
| `ES_CCC` | Es CCO? | Booleano | 1 | | Indica si la cuenta es de tipo complementario o analítico. |

## 🔍 Índices

| Identificador | Nombre | Tipo de índice |
| :--- | :--- | :--- |
| `ID` | Código | Clave única |
| `NAME` | Alfabético | Acepta repetidas |
| `WORDS` | Palabras | Palabras |
| `PARTS` | Trozos de palabras | Trozos de palabras |
| `CTA_SAT` | Código Cuenta | Clave única |
| `PAD_CTA` | Cuenta Padre | Acepta repetidas |

## 🔗 Enlaces Plurales

| Identificador | Nombre | Tabla enlazada | Índice |
| :--- | :--- | :--- | :--- |
| `002_ASI_CTA_CON` | 002: Asientos | `002_ASI@datta_erp_dat` | `CTA_CON` |
| `002_CTA_CNT_PAD_CTA` | 002: Cuentas Contables | `002_CTA_CNT@datta_erp_dat` | `PAD_CTA` |
| `002_SAL_SALDO_UNI` | 002: Saldos | `002_SAL@datta_erp_dat` | `SALDO_UNI` |

## ⚡ Triggers
*No reporta triggers.*

## 📌 Notas
La naturaleza de la cuenta (`NAT_CTA`) es vital para los procesos de cierre y generación de estados financieros. Una cuenta solo debe permitir asientos si es de "último nivel". El enlace plural hacia `002_SAL` garantiza la trazabilidad de los acumulados mensuales por cuenta. Se recomienda bloquear la edición de `CTA_SAT` una vez que la cuenta tiene movimientos para evitar inconsistencias en reportes al SAT.
