# Tabla: 003: Participantes

Tipo: Maestro
Reside en: Disco
Longitud del registro: 22
Número de campos: 7
Número de indices: 3

## 🛠️ Estructura de Campos

| Identificador | Nombre | Tipo | Longitud | Tipo de enlace |
| :--- | :--- | :--- | :--- | :--- |
| `ID` | Código | Numérico | 3 | |
| `CFDI_PAR` | CFDI Participantes | Numérico | 3 | Maestro: 003__CFDI_TRA@datta_erp_dat |
| `ENT_PAR` | Entidad Participante | Numérico | 3 | Maestro: 001__ENT_MTO@datta_erp_dat |
| `ROL` | Rol | Alfa 256 | 1 | |
| `REG_FIS_PAR` | Regimen Fiscal Participante | Alfa 256 | 3 | |
| `USO_CFDI` | Uso de CFDI | Alfa 256 | 3 | |
| `CP_PAR` | Codigo Postal | Alfa 256 | 5 | |

## 🔍 Índices

| Identificador | Nombre | Tipo de indice |
| :--- | :--- | :--- |
| `ID` | Código | Clave única |
| `CFDI_PAR` | CFDI Participantes | Acepta repetidas |
| `ENT_PAR` | Entidad Participante | Acepta repetidas |
