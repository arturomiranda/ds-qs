# Tabla: ID: 003_IMP_RET
# Nombre: 003: Impuestos Retenidos

## 📄 Información General
- Tipo de tabla: Maestro
- Reside en: Disco
- Longitud del registro: 43
- Número de campos: 7
- Número de índices: 2

## 📝 Descripción
Detalle granular de impuestos retenidos por comprobante fiscal. Almacena cada retención aplicada (ISR, IVA Retenido, IEPS Retenido) con su base gravable, tasa y monto. Cumple con la normativa fiscal de desglose de impuestos retenidos por tipo, permitiendo que el ERP genere la información necesaria para el nodo `Retenciones` del XML de acuerdo al Anexo 20.

## 🛠️ Estructura de Campos

| Identificador | Nombre | Tipo | Longitud | Tipo de enlace | Comentarios y Relaciones |
| :--- | :--- | :--- | :--- | :--- | :--- |
| `ID` | Código | Numérico | 3 | | ID único del registro de retención. |
| `CFDI_PAD` | CFDI Rel. | Numérico | 3 | Maestro: `003_CFDI@datta_erp_dat` | Enlace al CFDI de cabecera. |
| `CLV_IMP_SAT` | Impuesto SAT | Alfa 256 | 3 | | Clave del impuesto retenido (001: ISR, 002: IVA). |
| `TIP_FAC_SAT` | Tipo Factor SAT | Alfa 256 | 3 | | Tasa, Cuota o Exento. |
| `TAS_CUO` | Tasa o Cuota | Numérico | 10 | | Valor de la tasa aplicada (ej. 0.106666). |
| `NUM_IMP` | Importe | Numérico | 10 | | Monto total de la retención calculada. |
| `NUM_BASE` | Base | Numérico | 10 | | Importe sobre el cual se aplica la retención. |

## 🔍 Índices

| Identificador | Nombre | Tipo de índice |
| :--- | :--- | :--- |
| `ID` | Código | Clave única |
| `CFDI_PAD` | CFDI Rel. PAD | Acepta repetidas |

## 🔗 Enlaces Plurales
*No reporta enlaces plurales.*

## ⚡ Triggers
- **Actualización:** Notifica cambios a la tabla resumen `003_IMP`.

## 📌 Notas
Es vital que el campo `TAS_CUO` mantenga el número de decimales requerido por el SAT para evitar discrepancias de centavos. La `NUM_BASE` suele ser el subtotal de la factura o de un concepto específico. Esta tabla es consultada directamente por el motor de timbrado para construir el desglose impositivo legal.
