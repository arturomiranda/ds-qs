# Tabla: 002: Centros de Costos

Tipo: Maestro
Reside en: Disco
Longitud del registro: 127
Número de campos: 4
Número de indices: 6

## 🛠️ Estructura de Campos

| Identificador | Nombre | Tipo | Longitud | Tipo de enlace |
| :--- | :--- | :--- | :--- | :--- |
| `ID` | Código | Numérico | 3 | |
| `NAME` | Nombre CCO | Alfa 256 | 100 | |
| `COD_CCO` | Codigo Centro de Costo | Alfa 256 | 20 | |
| `PAD_CCO` | Centro Cos. Padre | Numérico | 3 | Maestro: 002__CCO_MTO@datta_erp_dat |

## 🔍 Índices

| Identificador | Nombre | Tipo de indice |
| :--- | :--- | :--- |
| `ID` | Código | Clave única |
| `NAME` | Alfabético | Acepta repetidas |
| `WORDS` | Palabras | Palabras |
| `PARTS` | Trozos de palabras | Trozos de palabras |
| `COD_CCO` | Codigo Centro de Costo | Clave única |
| `PAD_CCO_REL` | Relacion Padre | Acepta repetidas |

## 🔗 Enlaces plurales

| Identificador | Nombre | Tabla enlazada | Índice |
| :--- | :--- | :--- | :--- |
| `002__CCO_MTO_PAD_CCO_REL` | 002: Centros de Costos | 002__CCO_MTO@datta_erp_dat | PAD_CCO_REL |
