# Tabla: 003: Impuestos Traslados

Tipo: Maestro
Reside en: Disco
Longitud del registro: 43
Número de campos: 7
Número de indices: 2

## 🛠️ Estructura de Campos

| Identificador | Nombre | Tipo | Longitud | Tipo de enlace |
| :--- | :--- | :--- | :--- | :--- |
| `ID` | Código | Numérico | 3 | |
| `CFDI_PAD` | CFDI Rel. | Numérico | 3 | Maestro: 003__CFDI_TRA@datta_erp_dat |
| `CLV_IMP_SAT` | Impuesto SAT | Alfa 256 | 3 | |
| `TIP_FAC_SAT` | Tipo Factor SAT | Alfa 256 | 3 | |
| `TAS_CUO` | Tasa o Cuota | Numérico | 10 | |
| `NUM_IMP` | Importe | Numérico | 10 | |
| `NUM_BASE` | Base | Numérico | 10 | |

## 🔍 Índices

| Identificador | Nombre | Tipo de indice |
| :--- | :--- | :--- |
| `ID` | Código | Clave única |
| `CFDI_PAD` | CFDI Rel. | Acepta repetidas |

## 🔄 Actualizaciones

| Identificador | Nombre | Campo enlazado |
| :--- | :--- | :--- |
| `CFDI_PAD` | | `CFDI_PAD` |
