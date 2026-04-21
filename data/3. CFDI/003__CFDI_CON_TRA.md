# Tabla: 003: Conceptos de CFDI

Tipo: Maestro
Reside en: Disco
Longitud del registro: 314
Número de campos: 9
Número de indices: 2

## 🛠️ Estructura de Campos

| Identificador | Nombre | Tipo | Longitud | Tipo de enlace |
| :--- | :--- | :--- | :--- | :--- |
| `ID` | Código | Numérico | 3 | |
| `CFDI_CON` | CFDI | Numérico | 3 | Maestro: 003__CFDI_TRA@datta_erp_dat |
| `COD_SAT` | Clave Prod/Serv SAT | Alfa 256 | 8 | |
| `CANT` | Cantidad | Numérico | 10 | |
| `CLV_UNI_SAT` | Clave Unidad | Alfa 256 | 3 | |
| `NOM_DES` | Descripción | Alfa 256 | 256 | |
| `VAL_UNI` | Valor Unitario | Numérico | 10 | |
| `IMPO` | Importe | Numérico | 10 | |
| `DESC` | Descuento | Numérico | 10 | |

## 🔍 Índices

| Identificador | Nombre | Tipo de indice |
| :--- | :--- | :--- |
| `ID` | Código | Clave única |
| `CFDI_CON` | CFDI | Acepta repetidas |

## 🔄 Actualizaciones

| Identificador | Nombre | Campo enlazado |
| :--- | :--- | :--- |
| `CFDI_CON` | | `CFDI_CON` |
