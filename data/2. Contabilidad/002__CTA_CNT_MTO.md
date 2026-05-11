	# Tabla: 002: Cuentas Contables

Tipo: Maestro
Reside en: Disco
Longitud del registro: 287
Número de campos: 7
Número de indices: 7

## 🛠️ Estructura de Campos

| Identificador | Nombre | Tipo | Longitud | Tipo de enlace |
| :--- | :--- | :--- | :--- | :--- |
| `ID` | Código | Numérico | 3 | |
| `NAME` | Nombre de la cuenta | Alfa 256 | 256 | |
| `PAD_CTA` | Cuenta Padre | Numérico | 3 | Maestro: 002__CTA_CNT_MTO@datta_erp_dat |
| `COD_CTA_SAT` | Código Cuenta | Alfa 256 | 20 | |
| `NAT_CTA` | Naturaleza de la Cuenta | Alfa 256 | 1 | |
| `TIP_CTA` | Tipo de la Cuenta | Alfa 256 | 1 | |
| `NIV_CTA` | Nivel de Cuenta | Numérico | 2 | |

## 🔍 Índices

| Identificador | Nombre | Tipo de indice |
| :--- | :--- | :--- |
| `ID` | Código | Clave única |
| `NAME` | Alfabético | Acepta repetidas |
| `WORDS` | Palabras | Palabras |
| `PARTS` | Trozos de palabras | Trozos de palabras |
| `COD_CTA_SAT` | Código Cuenta | Clave única |
| `PAD_CTA` | Cuenta Padre | Acepta repetidas |
| `TIP_CTA_FIL` | Filtro Tipo de la Cuenta | Acepta repetidas |

## 🔗 Enlaces plurales

| Identificador | Nombre | Tabla enlazada | Índice |
| :--- | :--- | :--- | :--- |
| `002__ASI_TRA_CTA_CON` | 002: Asientos | 002__ASI_TRA@datta_erp_dat | CTA_CON |
| `002__CTA_CNT_MTO_PAD_CTA` | 002: Cuentas Contables | 002__CTA_CNT_MTO@datta_erp_dat | PAD_CTA |
| `002__SAL_TRA_SALDO_UNI` | 002: Saldos | 002__SAL_TRA@datta_erp_dat | SALDO_UNI |
