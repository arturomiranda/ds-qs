# Tabla: Tabla Impuestos

Tipo: Maestro
Reside en: Disco
Longitud del registro: 82
Número de campos: 7
Número de índices: 4

## 🛠️ Estructura de Campos

| Identificador | Nombre | Tipo | Longitud | Tipo de enlace |
| :--- | :--- | :--- | :--- | :--- |
| `ID` | Código | Numérico | 3 | |
| `NAME` | Nombre | Alfa 256 | 35 | |
| `IMP_RET` | Retencion | Booleano | 1 | |
| `IMP_TRA` | Traslado | Booleano | 1 | |
| `LOC_FED` | Nombre (Local/Federal) | Alfa 256 | 35 | |
| `FEC_INI_VIG` | Fecha Inicio de Vigencia | Fecha | 3 | |
| `FEC_FIN_VIG` | Fecha Fin de Vigencia | Fecha | 3 | |

## 🔍 Índices

| Identificador | Nombre | Tipo de indice |
| :--- | :--- | :--- |
| `ID` | Código | Clave única |
| `NAME` | Alfabético | Acepta repetidas |
| `WORDS` | Palabras | Palabras |
| `PARTS` | Trozos de palabras | Trozos de palabras |