# Tabla: 001: Grupos de Usuarios

Esta tabla define los grupos o roles de seguridad del sistema. Es la base para la asignación de permisos colectivos.

## 📄 Información General
- **ID de Tabla:** `001_GRP_USU`
- **Tipo:** Maestro
- **Reside en:** Disco
- **Longitud del registro:** 101
- **Número de campos:** 10
- **Número de índices:** 4
- **Descripción:** Definición de grupos de seguridad para la gestión de usuarios y permisos.

## 🛠️ Estructura de Campos

| Identificador | Nombre | Tipo | Longitud | Tipo de enlace |
| :--- | :--- | :--- | :--- | :--- |
| `ID` | Código | Numérico | 3 | |
| `NAME` | Nombre del Grupo | Alfa 128 | 35 | |
| `DESC` | Descripción | Alfa 256 | 40 | |
| `CRD_USR` | Creado por | Numérico | 3 | Maestro: 001_USU@datta_erp_dat |
| `CRD_TIM` | Creado el | Tiempo | 4 | |
| `MDF_USR` | Modificado por | Numérico | 3 | Maestro: 001_USU@datta_erp_dat |
| `MDF_TIM` | Modificado el | Tiempo | 4 | |
| `OFF` | Desactivado | Booleano | 1 | |
| `OFF_USR` | Desactivado por | Numérico | 3 | Maestro: 001_USU@datta_erp_dat |
| `OFF_TIM` | Desactivado el | Tiempo | 4 | |

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
| `001_PRM_GRP` | 001: Permisos | 001_PRM@datta_erp_dat | GRP |
| `001_PRM_GRP_TIP_OBJ` | 001: Permisos: Permiso por Grupo y Objeto | 001_PRM@datta_erp_dat | GRP_TIP_OBJ |
| `001_USU_GRP_GRP` | 001: Usuarios por Grupo | 001_USU_GRP@datta_erp_dat | GRP |

## 📝 Notas
- Un grupo agrupa a múltiples usuarios para facilitar la administración de permisos.
- La tabla incluye trazabilidad completa y control de estado (`OFF`).
