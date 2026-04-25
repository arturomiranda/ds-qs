# Tabla: Tabla Número Autorización Naviero

Tipo: Maestro
Reside en: Disco
Longitud del registro: 266
Número de campos: 4
Número de indices: 4

## 🛠️ Estructura de Campos

| Identificador | Nombre | Tipo | Longitud | Tipo de enlace |
| :--- | :--- | :--- | :--- | :--- |
| `ID` | Código | Numérico | 3 |  |
| `NUM_AUT_NAV` | Nombre | Alfa 256 | 256 |  |
| `FEC_INI_VIG` | Fecha de inicio de vigencia | Fecha | 3 |  |
| `FEC_FIN_VIG` | Fecha de fin de vigencia | Fecha | 3 |  |

## 🔍 Índices

| Identificador | Nombre | Tipo de indice |
| :--- | :--- | :--- |
| `ID` | Código | Clave única |
| `NAME` | Alfabético | Acepta repetidas |
| `WORDS` | Palabras | Palabras |
| `PARTS` | Trozos de palabras | Trozos de palabras |
