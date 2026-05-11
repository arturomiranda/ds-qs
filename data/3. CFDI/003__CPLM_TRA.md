# Tabla: 003: Complementos

Tipo: Maestro
Reside en: Disco
Longitud del registro: 105
Número de campos: 5
Número de indices: 2

## 🛠️ Estructura de Campos

| Identificador | Nombre           | Tipo         | Longitud | Tipo de enlace                       |
| :------------ | :--------------- | :----------- | :------- | :----------------------------------- |
| `ID`          | Código           | Numérico     | 3        |                                      |
| `CFDI_CPLM`   | CFDI             | Numérico     | 3        | Maestro: 003__CFDI_TRA@datta_erp_dat |
| `TIP_CPLM`    | Tipo Complemento | Alfa 256     | 50       |                                      |
| `JSN_DAT`     | Datos JSON       | Objeto Texto | 8        |                                      |

## 🔍 Índices

| Identificador | Nombre | Tipo de indice |
| :--- | :--- | :--- |
| `ID` | Código | Clave única |
| `CFDI_CPLM` | CFDI | Acepta repetidas |
