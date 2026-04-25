# Tabla: Tabla Códigos Postales

Tipo: Maestro
Reside en: Disco
Longitud del registro: 21
Número de campos: 6
Número de indices: 4

## 🛠️ Estructura de Campos

| Identificador | Nombre | Tipo | Longitud | Tipo de enlace |
| :--- | :--- | :--- | :--- | :--- |
| `ID` | Código | Alfa 256 | 5 | |
| `EST` | Estado | Alfa 256 | 3 | Maestro: A20_EST@catalogos_sat_dat |
| `MUN` | Municipio | Numérico | 3 | Maestro: A20_MUN@catalogos_sat_dat |
| `LOC` | Localidad | Numérico | 3 | Maestro: A20_LOC@catalogos_sat_dat |
| `FEC_INI_VIG` | Fecha de inicio de vigencia | Fecha | 3 | |
| `FEC_FIN_VIG` | Fecha de fin de vigencia | Fecha | 3 | |

## 🔍 Índices

| Identificador | Nombre | Tipo de indice |
| :--- | :--- | :--- |
| `ID` | Código | Clave única |
| `EST` | Estado | Acepta repetidas |
| `MUN` | Municipio | Acepta repetidas |
| `LOC` | Localidad | Acepta repetidas |

## 🔗 Enlaces plurales

| Identificador | Nombre | Tabla enlazada | Índice |
| :--- | :--- | :--- | :--- |
| `A20_COL_CP` | ANEXO 20: Tabla Colonias | A20_COL@catalogos_sat_dat | CP |