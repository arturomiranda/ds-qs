# Tabla: 001: Monedas

Tipo: Maestro
Reside en: Disco
Longitud del registro: 84
Número de campos: 4
Número de indices: 5

## 🛠️ Estructura de Campos

| Identificador | Nombre | Tipo | Longitud | Tipo de enlace |
| :--- | :--- | :--- | :--- | :--- |
| `ID` | Código | Numérico | 3 | |
| `NAME` | Nombre Moneda | Alfa 128 | 35 | |
| `COD_MON_SAT` | Cod. Moneda SAT | Alfa 256 | 40 | Maestro |
| `SIM_MON` | Simbolo Moneda | Alfa 256 | 5 | |

## 🔍 Índices

| Identificador | Nombre | Tipo de indice |
| :--- | :--- | :--- |
| `ID` | Código | Clave única |
| `NAME` | Alfabético | Acepta repetidas |
| `WORDS` | Palabras | Palabras |
| `PARTS` | Trozos de palabras | Trozos de palabras |
| `COD_MON_SAT` | Cod. Moneda SAT | Clave única |

## 🔗 Enlaces plurales

| Identificador | Nombre | Tabla enlazada | Índice |
| :--- | :--- | :--- | :--- |
| `001_TDC_MTO_MON_FEC` | 001: Tipos de Cambios | 001__TDC_MTO@datta_erp_dat | MON_FEC |
| `002_POL_TRA_REL_MON_POL` | 002: Pólizas | 002_POL_TRA@datta_erp_dat | REL_MON_POL |
