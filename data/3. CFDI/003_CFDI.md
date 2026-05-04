# Tabla: 003: CFDI

Esta es la tabla maestra principal de Comprobantes Fiscales Digitales por Internet (CFDI). Almacena los datos generales del comprobante, el folio fiscal (UUID) y el estado del timbrado.

## 📄 Información General
- **ID de Tabla:** `003_CFDI`
- **Tipo:** Maestro
- **Reside en:** Disco
- **Longitud del registro:** 223
- **Número de campos:** 27
- **Número de índices:** 4
- **Descripción:** Repositorio central de comprobantes fiscales digitales.

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
| `CRD_USR` | Creado por | Numérico | 3 | Maestro: 001_USU@datta_erp_dat |
| `CRD_TIM` | Creado el | Tiempo | 4 | |
| `MDF_USR` | Modificado por | Numérico | 3 | Maestro: 001_USU@datta_erp_dat |
| `MDF_TIM` | Modificado el | Tiempo | 4 | |
| `TMB` | Timbrado | Booleano | 1 | |
| `TMB_USR` | Timbrado por | Numérico | 3 | Maestro: 001_USU@datta_erp_dat |
| `TMB_TIM` | Timbrado el | Tiempo | 4 | |
| `CNC` | Cancelado | Booleano | 1 | |
| `CNC_USR` | Cancelado por | Numérico | 3 | Maestro: 001_USU@datta_erp_dat |
| `CNC_TIM` | Cancelado el | Tiempo | 4 | |

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
| `003_CFDIS_REL_CFDI_PAD` | 003: CFDIS Relacionados | 003_CFDIS_REL@datta_erp_dat | CFDI_PAD |
| `003_CFDI_CON_CFDI_CON` | 003: Conceptos de CFDI | 003_CFDI_CON@datta_erp_dat | CFDI_CON |
| `003_CPLM_CFDI_CPLM` | 003: Complementos | 003_CPLM@datta_erp_dat | CFDI_CPLM |
| `003_IMP_CFDI_IMP` | 003: Impuestos | 003_IMP@datta_erp_dat | CFDI_IMP |
| `003_IMP_RET_CFDI_PAD` | 003: Impuestos Retenidos | 003_IMP_RET@datta_erp_dat | CFDI_PAD |
| `003_IMP_TRSL_CFDI_PAD` | 003: Impuestos Trasladados | 003_IMP_TRSL@datta_erp_dat | CFDI_PAD |
| `003_PAR_CFDI` | 003: Participantes | 003_PAR@datta_erp_dat | CFDI |

## 📝 Notas
- El campo `UUID` es la clave única proporcionada por el PAC tras el timbrado exitoso.
- Los campos booleanos `TMB` y `CNC` permiten rastrear el estado actual del comprobante.
- Incluye trazabilidad de creación, modificación, timbrado y cancelación.
