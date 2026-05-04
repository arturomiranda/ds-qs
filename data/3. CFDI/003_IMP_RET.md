# Tabla: 003: Impuestos Retenidos

Esta tabla detalla los impuestos retenidos (ISR, IVA Retenido) aplicados a un CFDI, ya sea a nivel global o por concepto.

## 📄 Información General
- **ID de Tabla:** `003_IMP_RET`
- **Tipo:** Maestro
- **Reside en:** Disco
- **Longitud del registro:** 43
- **Número de campos:** 7
- **Número de índices:** 2
- **Descripción:** Detalle de impuestos retenidos por comprobante fiscal.

## 🛠️ Estructura de Campos

| Identificador | Nombre | Tipo | Longitud | Tipo de enlace |
| :--- | :--- | :--- | :--- | :--- |
| `ID` | Código | Numérico | 3 | |
| `CFDI_PAD` | CFDI Rel. | Numérico | 3 | Maestro: 003_CFDI@datta_erp_dat |
| `CLV_IMP_SAT` | Impuesto SAT | Alfa 256 | 3 | |
| `TIP_FAC_SAT` | Tipo Factor SAT | Alfa 256 | 3 | |
| `TAS_CUO` | Tasa o Cuota | Numérico | 10 | |
| `NUM_IMP` | Importe | Numérico | 10 | |
| `NUM_BASE` | Base | Numérico | 10 | |

## 🔍 Índices

| Identificador | Nombre | Tipo de indice |
| :--- | :--- | :--- |
| `ID` | Código | Clave única |
| `CFDI_PAD` | CFDI Rel. PAD | Acepta repetidas |

## 📝 Notas
- Los campos `CLV_IMP_SAT` y `TIP_FAC_SAT` utilizan las claves oficiales del catálogo del SAT.
- La base (`NUM_BASE`) es el importe sobre el cual se calcula la retención.
