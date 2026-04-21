	# Tabla: 003: CFDI

Tipo: Maestro
Reside en: Disco
Longitud del registro: 193
Número de campos: 17
Número de indices: 4

## 🛠️ Estructura de Campos

| Identificador | Nombre | Tipo | Longitud | Tipo de enlace |
| :--- | :--- | :--- | :--- | :--- |
| `ID` | Código | Numérico | 3 | |
| `SER` | Serie | Alfa 256 | 40 | |
| `FOL` | Folio | Alfa 256 | 40 | |
| `FECH_EMI` | Fecha | Fecha | 3 | |
| `FRM_PAG` | Forma de Pago | Numérico | 3 | |
| `SUB_TOT` | SubTotal | Numérico | 10 | |
| `DESC` | Descuento | Numérico | 10 | |
| `MON_SAT` | Moneda | Alfa 256 | 3 | |
| `TOTL` | TOTAL | Numérico | 9 | |
| `TIP_COM` | Tipo Comprobante | Alfa 256 | 1 | |
| `MET_PAG` | Metodo de Pago | Alfa 256 | 3 | |
| `ENT_EMI` | Entidad Emisora | Numérico | 3 | |
| `ENT_REC` | Entidad Receptora Cliente | Numérico | 3 | |
| `USO_CFDI` | Uso de CFDI | Alfa 256 | 3 | |
| `UUID` | Folio Fiscal | Alfa 40 | 40 | |
| `XML_TIM` | Xml Timbrado | Objeto Texto | 8 | |
| `VAL_TDC` | Valor Tipo de Cambio | Numérico | 10 | |

## 🔍 Índices

| Identificador | Nombre | Tipo de indice |
| :--- | :--- | :--- |
| `ID` | Código | Clave única |
| `UUID` | Folio Fiscal | Clave única |
| `FECH_ORD` | Fecha | Acepta repetidas |
| `SER_FOL` | Serie y Folio | Acepta repetidas |

## 🔗 Enlaces plurales

| Identificador | Nombre | Tabla enlazada | Índice |
| :--- | :--- | :--- | :--- |
| `003__CFDIS_REL_TRA_CFDI_PAD` | 003: CFDIS Relacionados | 003__CFDIS_REL_TRA@datta_erp_dat | CFDI_PAD |
| `003__CFDI_CON_TRA_CFDI_CON` | 003: Conceptos de CFDI | 003__CFDI_CON_TRA@datta_erp_dat | CFDI_CON |
| `003__CPLM_TRA_CFDI_CPLM` | 003: Complementos | 003__CPLM_TRA@datta_erp_dat | CFDI_CPLM |
| `003__IMP_RET_TRA_CFDI_PAD` | 003: Impuestos Retenidos | 003__IMP_RET_TRA@datta_erp_dat | CFDI_PAD |
| `003__IMP_TRA_CFDI_IMP` | 003: Impuestos | 003__IMP_TRA@datta_erp_dat | CFDI_IMP |
| `003__IMP_TRSL_TRA_CFDI_PAD` | 003: Impuestos Traslados | 003__IMP_TRSL_TRA@datta_erp_dat | CFDI_PAD |
| `003__PAR_TRA_CFDI_PAR` | 003: Participantes | 003__PAR_TRA@datta_erp_dat | CFDI_PAR |
