# Tabla: 001: Telefonos

Tipo: Maestro
Reside en: Disco
Longitud del registro: 22
Número de campos: 4
Número de indices: 4

## 🛠️ Estructura de Campos

| Identificador | Nombre | Tipo | Longitud | Tipo de enlace |
| :--- | :--- | :--- | :--- | :--- |
| `ID` | Código | Numérico | 3 | |
| `ENT_TEL` | Entidad Telefono | Numérico | 3 | Maestro: 001__ENT_MTO@datta_erp_dat |
| `NUM_TEL` | Numero Telefonico | Alfa 256 | 14 | |
| `TIP_TEL` | Tipo de Telefono | Alfa 256 | 1 | |

## 🔍 Índices

| Identificador | Nombre | Tipo de indice |
| :--- | :--- | :--- |
| `ID` | Código | Clave única |
| `REL_ENT_TEL` | Por Entidad | Acepta repetidas |
| `PAL_BUS_TEL` | Búsqueda por Palabras Tel. | Palabras |
| `TRZ_BUS_TEL` | Búsqueda por Palabras Tel. | Trozos de palabras |
