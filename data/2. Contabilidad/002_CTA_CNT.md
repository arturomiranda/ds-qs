# Tabla: 002: Cuentas Contables

Esta tabla maestra contiene el catálogo de cuentas contables del sistema. Soporta una estructura jerárquica para la consolidación financiera.

## 📄 Información General
- **ID de Tabla:** `002_CTA_CNT`
- **Tipo:** Maestro
- **Reside en:** Disco
- **Longitud del registro:** 288
- **Número de campos:** 8
- **Número de índices:** 6
- **Descripción:** Catálogo maestro de cuentas contables y su jerarquía.

## 🛠️ Estructura de Campos

| Identificador | Nombre | Tipo | Longitud | Tipo de enlace |
| :--- | :--- | :--- | :--- | :--- |
| `ID` | Código | Numérico | 3 | |
| `NAME` | Nombre de la cuenta | Alfa 256 | 256 | |
| `PAD_CTA` | Cuenta Padre | Numérico | 3 | Maestro: 002_CTA_CNT@datta_erp_dat |
| `CTA_SAT` | Cuenta SAT | Alfa 256 | 20 | |
| `NAT_CTA` | Naturaleza | Alfa 256 | 1 | |
| `NIV_CTA` | Nivel | Numérico | 2 | |
| `REG_CCO` | Registra CCO | Booleano | 1 | |
| `ES_CCC` | Es CCO? | Booleano | 1 | |

## 🔍 Índices

| Identificador | Nombre | Tipo de indice |
| :--- | :--- | :--- |
| `ID` | Código | Clave única |
| `NAME` | Alfabético | Acepta repetidas |
| `WORDS` | Palabras | Palabras |
| `PARTS` | Trozos de palabras | Trozos de palabras |
| `CTA_SAT` | Código Cuenta | Clave única |
| `PAD_CTA` | Cuenta Padre | Acepta repetidas |

## 🔗 Enlaces plurales

| Identificador | Nombre | Tabla enlazada | Índice |
| :--- | :--- | :--- | :--- |
| `002_ASI_CTA_CON` | 002: Asientos | 002_ASI@datta_erp_dat | CTA_CON |
| `002_CTA_CNT_PAD_CTA` | 002: Cuentas Contables (Subcuentas) | 002_CTA_CNT@datta_erp_dat | PAD_CTA |
| `002_SAL_SALDO_UNI` | 002: Saldos | 002_SAL@datta_erp_dat | SALDO_UNI |

## 📝 Notas
- La naturaleza (`NAT_CTA`) define si la cuenta es Deudora (D) o Acreedora (A).
- El campo `REG_CCO` indica si la cuenta obliga a capturar un Centro de Costo en los asientos.
- El campo `CTA_SAT` vincula la cuenta con el agrupador oficial para la contabilidad electrónica.
