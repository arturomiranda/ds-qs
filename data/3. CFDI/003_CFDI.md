# Tabla: ID: 003_CFDI

## 📄 Información General
- Tipo de tabla: Maestro
- Reside en: Disco
- Longitud del registro: 223
- Número de campos: 27
- Número de índices: 4

## 📝 Descripción
Esta es la tabla maestra central para la facturación electrónica en México (CFDI). Almacena las cabeceras de los comprobantes fiscales, incluyendo folios internos, estados de timbrado, totales y los identificadores únicos universales (UUID) proporcionados por el SAT. Es el nexo de unión entre la operación comercial y el cumplimiento fiscal, conectando con clientes (`001_ENT`), pólizas contables (`002_POL`) y desgloses de impuestos.

## 🛠️ Estructura de Campos

| Identificador | Nombre | Tipo | Longitud | Tipo de enlace | Comentarios y Relaciones |
| :--- | :--- | :--- | :--- | :--- | :--- |
| `ID` | Código | Numérico | 3 | | ID interno del comprobante. |
| `SER` | Serie | Alfa 256 | 40 | | Serie interna de facturación (ej. "A", "F"). |
| `FOL` | Folio | Alfa 256 | 40 | | Folio consecutivo interno. |
| `FECH_EMI` | Fecha | Fecha | 3 | | Fecha y hora de emisión del comprobante. |
| `FRM_PAG` | Forma de Pago | Numérico | 3 | | Enlace a catálogo SAT (ej. 01: Efectivo, 03: Transferencia). |
| `SUB_TOT` | SubTotal | Numérico | 10 | | Importe antes de descuentos e impuestos. |
| `DESC` | Descuento | Numérico | 10 | | Importe total de descuentos aplicados. |
| `MON_SAT` | Moneda | Alfa 256 | 3 | | Código ISO de la moneda (MXN, USD). |
| `TOTL` | TOTAL | Numérico | 9 | | Importe final del comprobante. |
| `TIP_COM` | Tipo Comprobante | Alfa 256 | 1 | | I: Ingreso, E: Egreso, P: Pago, T: Traslado, N: Nómina. |
| `MET_PAG` | Metodo de Pago | Alfa 256 | 3 | | PUE (Pago en una sola exhibición) o PPD (Pago en parcialidades). |
| `ENT_EMI` | Entidad Emisora | Numérico | 3 | | Enlace a `001_ENT` (La empresa emisora). |
| `ENT_REC` | Entidad Receptora Cliente | Numérico | 3 | | Enlace a `001_ENT` (El cliente receptor). |
| `USO_CFDI` | Uso de CFDI | Alfa 256 | 3 | | Clave de uso fiscal declarada por el receptor (G03, P01, etc.). |
| `UUID` | Folio Fiscal | Alfa 40 | 40 | | Identificador único del SAT tras el timbrado. |
| `XML_TIM` | Xml Timbrado | Objeto Texto | 8 | | Almacena el contenido completo del XML certificado. |
| `VAL_TDC` | Valor Tipo de Cambio | Numérico | 10 | | Paridad utilizada en la transacción. |
| `CRD_USR` | Creado por | Numérico | 3 | Maestro: `001_USU@datta_erp_dat` | Auditoría de creación. |
| `CRD_TIM` | Creado el | Tiempo | 4 | | Timestamp de creación. |
| `MDF_USR` | Modificado por | Numérico | 3 | Maestro: `001_USU@datta_erp_dat` | Auditoría de edición. |
| `MDF_TIM` | Modificado el | Tiempo | 4 | | Timestamp de edición. |
| `TMB` | Timbrado | Booleano | 1 | | Bandera que indica si el documento ya fue certificado por el PAC. |
| `TMB_USR` | Timbrado por | Numérico | 3 | Maestro: `001_USU@datta_erp_dat` | Usuario que ejecutó la acción de timbrado. |
| `TMB_TIM` | Timbrado el | Tiempo | 4 | | Momento exacto de la certificación. |
| `CNC` | Cancelado | Booleano | 1 | | Indica si el CFDI ha sido invalidado. |
| `CNC_USR` | Cancelado por | Numérico | 3 | Maestro: `001_USU@datta_erp_dat` | Usuario que ejecutó la cancelación. |
| `CNC_TIM` | Cancelado el | Tiempo | 4 | | Momento de la cancelación. |

## 🔍 Índices

| Identificador | Nombre | Tipo de índice |
| :--- | :--- | :--- |
| `ID` | Código | Clave única |
| `UUID` | Folio Fiscal | Clave única |
| `FECH_ORD` | Fecha | Acepta repetidas |
| `SER_FOL` | Serie y Folio | Acepta repetidas |

## 🔗 Enlaces Plurales

| Identificador | Nombre | Tabla enlazada | Índice |
| :--- | :--- | :--- | :--- |
| `003_CFDIS_REL_CFDI_PAD` | 003: CFDIS Relacionados | `003_CFDIS_REL@datta_erp_dat` | `CFDI_PAD` |
| `003_CFDI_CON_CFDI_CON` | 003: Conceptos de CFDI | `003_CFDI_CON@datta_erp_dat` | `CFDI_CON` |
| `003_CPLM_CFDI_CPLM` | 003: Complementos | `003_CPLM@datta_erp_dat` | `CFDI_CPLM` |
| `003_IMP_CFDI_IMP` | 003: Impuestos | `003_IMP@datta_erp_dat` | `CFDI_IMP` |
| `003_IMP_RET_CFDI_PAD` | 003: Impuestos Retenidos | `003_IMP_RET@datta_erp_dat` | `CFDI_PAD` |
| `003_IMP_TRSL_CFDI_PAD` | 003: Impuestos Trasladados | `003_IMP_TRSL@datta_erp_dat` | `CFDI_PAD` |
| `003_PAR_CFDI` | 003: Participantes | `003_PAR@datta_erp_dat` | `CFDI` |

## ⚡ Triggers
- **Alta:** Anterior a un alta de ficha
- **Modificación:** Anterior a una modificación de ficha

## 📌 Notas
El campo `XML_TIM` es crítico para la recuperación del documento legal. La tabla implementa una trazabilidad exhaustiva no solo de quién creó el registro, sino de quién lo timbró y quién lo canceló. El índice único en `UUID` previene la duplicidad fiscal en el sistema. Se recomienda que el trigger de modificación bloquee cualquier cambio en campos base (totales, emisores, receptores) una vez que la bandera `TMB` es Verdadera.
