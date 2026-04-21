# Tabla: 003: CFDIS Relacionados

Tipo: Maestro
Reside en: Disco
Longitud del registro: 49
Número de campos: 4
Número de indices: 2

## 🛠️ Estructura de Campos

| Identificador | Nombre | Tipo | Longitud | Tipo de enlace |
| :--- | :--- | :--- | :--- | :--- |
| `ID` | Código | Numérico | 3 | |
| `CFDI_PAD` | CFDI Padre | Numérico | 3 | Maestro: 003__CFDI_TRA@datta_erp_dat |
| `UUID_REL` | UUID Relacionado | Alfa 256 | 40 | |
| `TIP_REL` | Tipo de Relación | Alfa 256 | 2 | |

## 🔍 Índices

| Identificador | Nombre | Tipo de indice |
| :--- | :--- | :--- |
| `ID` | Código | Clave única |
| `CFDI_PAD` | CFDI Padre | Acepta repetidas |
