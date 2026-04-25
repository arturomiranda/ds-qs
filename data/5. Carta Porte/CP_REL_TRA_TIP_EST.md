# Tabla:  Tabla Relación Transporte Tipo Estación

Tipo: Maestro
Reside en: Disco
Longitud del registro: 14
Número de campos: 5
Número de indices: 3

## 🛠️ Estructura de Campos

| Identificador | Nombre | Tipo | Longitud | Tipo de enlace |
| :--- | :--- | :--- | :--- | :--- |
| `ID` | Código | Numérico | 3 | |
| `EST` | Estación | Alfa 256 | 2 | Maestro: CP_TIP_EST@catalogos_sat_dat |
| `TRAN` | Transporte | Alfa 256 | 2 | Maestro: CP_CLV_TRAN@catalogos_sat_dat |
| `FEC_INI_VIG` | Fecha de inicio de vigencia | Fecha | 3 | |
| `FEC_FIN_VIG` | Fecha de fin de vigencia | Fecha | 3 | |

## 🔍 Índices

| Identificador | Nombre | Tipo de indice |
| :--- | :--- | :--- |
| `ID` | Código | Clave única |
| `EST` | Estacion | Acepta repetidas |
| `TRAN` | Transporte | Acepta repetidas |
