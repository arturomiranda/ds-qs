# Tabla: 001: Direcciones

Tipo: Maestro
Reside en: Disco
Longitud del registro: 607
Número de campos: 11
Número de indices: 4

## 🛠️ Estructura de Campos

| Identificador | Nombre | Tipo | Longitud | Tipo de enlace |
| :--- | :--- | :--- | :--- | :--- |
| `ID` | Código | Numérico | 3 | |
| `ENT_DIR` | Direccion Entidad | Numérico | 3 | Maestro: 001__ENT_MTO@datta_erp_dat |
| `TIP_DIR` | Tipo de Dirección | Alfa 256 | 1 | |
| `CLLE` | Calle | Alfa 256 | 254 | |
| `NUM_EXT_DIR` | Num. Exterior | Alfa 256 | 20 | |
| `NUM_INT_DIR` | Num. Interior | Alfa 256 | 20 | |
| `COL_DIR` | Colonia | Alfa 256 | 100 | |
| `CP_DIR` | Código Postal | Alfa 256 | 5 | |
| `CIU_DIR` | Ciudad | Alfa 256 | 100 | |
| `EST_DIR` | Estado | Alfa 256 | 50 | |
| `PAIS_DIR` | Pais | Alfa 256 | 50 | |

## 🔍 Índices

| Identificador | Nombre | Tipo de indice |
| :--- | :--- | :--- |
| `ID` | Código | Clave única |
| `REL_ENT_DIR` | Por Entidad | Acepta repetidas |
| `DIR_FIS` | Direccion Fiscal | Clave única |
| `CP_BUS` | Busqueda Código Postal | Acepta repetidas |