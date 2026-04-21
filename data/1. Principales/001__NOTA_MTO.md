# Tabla: 001: Notas

Tipo: Maestro
Reside en: Disco
Longitud del registro: 266
Número de campos: 4
Número de indices: 3

## 🛠️ Estructura de Campos

| Identificador | Nombre | Tipo | Longitud | Tipo de enlace |
| :--- | :--- | :--- | :--- | :--- |
| `ID` | Código | Numérico | 3 | |
| `ENT_NOT` | Notas de Entidad | Numérico | 3 | Maestro: 001__ENT_MTO@datta_erp_dat |
| `CON_NOTA` | Contenido de la nota | Alfa 256 | 256 | |
| `FECH` | Fecha | Fecha | 3 | |

## 🔍 Índices

| Identificador | Nombre | Tipo de indice |
| :--- | :--- | :--- |
| `ID` | Código | Clave única |
| `REL_ENT` | Por Entidad | Acepta repetidas |
| `FEC_ORD` | Orden Cronológico | Acepta repetidas |
