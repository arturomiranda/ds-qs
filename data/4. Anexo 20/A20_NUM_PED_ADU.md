# Tabla: Tabla Número Pedimiento Aduana

Tipo: Maestro
Reside en: Disco
Longitud del registro: 24
Número de campos: 7
Número de índices: 3

## 🛠️ Estructura de Campos

| Identificador | Nombre | Tipo | Longitud | Tipo de enlace |
| :--- | :--- | :--- | :--- | :--- |
| `ID` | Código | Numérico | 3 | |
| `ADU` | Aduana | Alfa 256 | 2 | Maestro: A20_ADU@catalogos_sat_dat |
| `PAT` | Patente | Alfa 256 | 4 | Maestro: A20_PAT_ADU@catalogos_sat_dat |
| `EJER` | Ejercicio | Alfa 256 | 4 | |
| `CANT` | Cantidad | Numérico | 4 | |
| `FEC_INI_VIG` | Fecha de inicio de vigencia | Fecha | 3 | |
| `FEC_FIN_VIG` | Fecha de fin de vigencia | Fecha | 3 | |

## 🔍 Índices

| Identificador | Nombre | Tipo de indice |
| :--- | :--- | :--- |
| `ID` | Código | Clave única |
| `PAT` | Patente | Acepta repetidas |
| `ADU` | Aduana | Acepta repetidas |