# Tabla: 003: Impuestos

Tipo: Maestro
Reside en: Disco
Longitud del registro: 27
Número de campos: 4
Número de indices: 2

## 🛠️ Estructura de Campos

| Identificador | Nombre | Tipo | Longitud | Tipo de enlace |
| :--- | :--- | :--- | :--- | :--- |
| `ID` | Código | Numérico | 3 | |
| `CFDI_IMP` | CFDI | Numérico | 3 | Maestro: 003__CFDI_TRA@datta_erp_dat |
| `TOT_TRAS` | Total Traslado | Numérico | 10 | |
| `CFDI_RET_TRAS` | Total Retención | Numérico | 10 | |

## 🔍 Índices

| Identificador | Nombre | Tipo de indice |
| :--- | :--- | :--- |
| `ID` | Código | Clave única |
| `CFDI_IMP` | CFDI | Acepta repetidas |
