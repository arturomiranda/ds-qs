# Tabla: 001: Perfiles

Tipo: Maestro
Reside en: Disco
Longitud del registro: 516
Número de campos: 3
Número de indices: 4

## 🛠️ Estructura de Campos

| Identificador | Nombre | Tipo | Longitud | Tipo de enlace |
| :--- | :--- | :--- | :--- | :--- |
| `ID` | Código | Numérico | 3 | |
| `NAME` | Nombre | Alfa 256 | 256 | |
| `DESC` | Descripcion | Alfa 256 | 256 | |

## 🔍 Índices

| Identificador | Nombre | Tipo de indice |
| :--- | :--- | :--- |
| `ID` | Código | Clave única |
| `NAME` | Alfabético | Acepta repetidas |
| `WORDS` | Palabras | Palabras |
| `PARTS` | Trozos de palabras | Trozos de palabras |

## 🔗 Enlaces plurales

| Identificador | Nombre | Tabla enlazada | Índice |
| :--- | :--- | :--- | :--- |
| `001_PRM_MTO_PRF_OBJ` | 001: Permisos | 001__PRM_MTO@datta_erp_dat | PRF_OBJ |
| `001_PRM_MTO_PRF_PRM` | 001: Permisos: Por Perfil | 001__PRM_MTO@datta_erp_dat | PRF_PRM |
| `001_USU_MTO_PER_REL_USU` | 001: Usuarios | 001__USU_MTO@datta_erp_dat | PER_REL_USU |
