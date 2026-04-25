# Tabla: Tabla Tipo Permiso

Tipo: Maestro
Reside en: Disco
Longitud del registro: 274
Número de campos: 6
Número de indices: 5

## 🛠️ Estructura de Campos

| Identificador | Nombre | Tipo | Longitud | Tipo de enlace |
| :--- | :--- | :--- | :--- | :--- |
| `ID` | Código | Numérico | 3 |  |
| `CLV` | Clave | Alfa 256 | 6 |  |
| `NAME` | Nombre | Alfa 256 | 256 |  |
| `TRAN` | Transporte | Alfa 256 | 2 | Maestro: CP_CLV_TRAN@catalogos_sat_dat |
| `FEC_INI_VIG` | Fecha de inicio de vigencia | Fecha | 3 |  |
| `FEC_FIN_VIG` | Fecha de fin de vigencia | Fecha | 3 |  |

## 🔍 Índices

| Identificador | Nombre | Tipo de indice |
| :--- | :--- | :--- |
| `ID` | Código | Clave única |
| `NAME` | Alfabético | Acepta repetidas |
| `WORDS` | Palabras | Palabras |
| `PARTS` | Trozos de palabras | Trozos de palabras |
| `TRAN` | Transporte | Acepta repetidas |
