# Tabla: 001: Correos

Tipo: Maestro
Reside en: Disco
Longitud del registro: 107
Número de campos: 3
Número de indices: 4

## 🛠️ Estructura de Campos

| Identificador | Nombre | Tipo | Longitud | Tipo de enlace |
| :--- | :--- | :--- | :--- | :--- |
| `ID` | Código | Numérico | 3 | |
| `ENT_EMA` | Entidad | Numérico | 3 | Maestro: 001__ENT_MTO@datta_erp_dat |
| `DIR_EMA` | Correo Electronico | Alfa 256 | 100 | |

## 🔍 Índices

| Identificador | Nombre | Tipo de indice |
| :--- | :--- | :--- |
| `ID` | Código | Clave única |
| `REL_ENT_EMA` | Por Entidad | Acepta repetidas |
| `PLB_EMA_BUS` | Busqueda Por Palabras Mail | Palabras |
| `TRZ_EMA_BUS` | Busqueda Por Trozos Mail | Trozos de palabras |
