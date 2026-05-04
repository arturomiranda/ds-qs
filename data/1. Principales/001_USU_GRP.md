# Tabla: 001: Usuarios por Grupo

Esta es una tabla de relación (muchos a muchos) que vincula a los usuarios con sus respectivos grupos de seguridad.

## 📄 Información General
- **ID de Tabla:** `001_USU_GRP`
- **Tipo:** Maestro
- **Reside en:** Disco
- **Longitud del registro:** 24
- **Número de campos:** 7
- **Número de índices:** 3
- **Descripción:** Vinculación entre la tabla de Usuarios y la tabla de Grupos.

## 🛠️ Estructura de Campos

| Identificador | Nombre | Tipo | Longitud | Tipo de enlace |
| :--- | :--- | :--- | :--- | :--- |
| `ID` | Código | Numérico | 3 | |
| `USU` | Usuario | Numérico | 3 | Maestro: 001_USU@datta_erp_dat |
| `GRP` | Grupo | Numérico | 3 | Maestro: 001_GRP_USU@datta_erp_dat |
| `CRD_USR` | Creado por | Numérico | 3 | Maestro: 001_USU@datta_erp_dat |
| `CRD_TIM` | Creado el | Tiempo | 4 | |
| `MDF_USR` | Modificado por | Numérico | 3 | Maestro: 001_USU@datta_erp_dat |
| `MDF_TIM` | Modificado el | Tiempo | 4 | |

## 🔍 Índices

| Identificador | Nombre | Tipo de indice |
| :--- | :--- | :--- |
| `ID` | Código | Clave única |
| `USU` | Usuario | Acepta repetidas |
| `GRP` | Grupo | Acepta repetidas |

## 📝 Notas
- Permite que un usuario pertenezca a uno o varios grupos de seguridad.
- Incluye trazabilidad para saber cuándo se asignó o modificó la pertenencia de un usuario a un grupo.
