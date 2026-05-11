# Tabla: ID: 001_GRP_USU

## 📄 Información General
- Tipo de tabla: Maestro
- Reside en: Disco
- Longitud del registro: 101
- Número de campos: 10
- Número de índices: 4

## 📝 Descripción
Esta tabla define los Grupos de Seguridad o Perfiles de Usuario del sistema. Su propósito es permitir una administración de permisos escalable; en lugar de asignar permisos a cada usuario individualmente, se asignan a un Grupo y los usuarios heredan estos privilegios al ser vinculados. Es el pilar del modelo RBAC (Role-Based Access Control) del ERP.

## 🛠️ Estructura de Campos

| Identificador | Nombre | Tipo | Longitud | Tipo de enlace | Comentarios y Relaciones |
| :--- | :--- | :--- | :--- | :--- | :--- |
| `ID` | Código | Numérico | 3 | | Clave primaria del grupo. |
| `NAME` | Nombre | Alfa 128 | 35 | | Nombre descriptivo del rol (ej. Contadores, Administradores). |
| `DESC` | Descripción | Alfa 256 | 40 | | Detalle de las responsabilidades o alcance del grupo. |
| `CRD_USR` | Creado por | Numérico | 3 | Maestro: `001_USU@datta_erp_dat` | Usuario que registró el grupo. |
| `CRD_TIM` | Creado el | Tiempo | 4 | | Fecha y hora de creación. |
| `MDF_USR` | Modificado por | Numérico | 3 | Maestro: `001_USU@datta_erp_dat` | Usuario que realizó el último cambio. |
| `MDF_TIM` | Modificado el | Tiempo | 4 | | Fecha y hora de modificación. |
| `OFF` | Desactivado | Booleano | 1 | | Permite inhabilitar todos los permisos de un grupo de golpe. |
| `OFF_USR` | Desactivado por | Numérico | 3 | Maestro: `001_USU@datta_erp_dat` | Usuario que ejecutó la desactivación. |
| `OFF_TIM` | Desactivado el | Tiempo | 4 | | Timestamp de la desactivación. |

## 🔍 Índices

| Identificador | Nombre | Tipo de índice |
| :--- | :--- | :--- |
| `ID` | Código | Clave única |
| `NAME` | Alfabético | Acepta repetidas |
| `WORDS` | Palabras | Palabras |
| `PARTS` | Trozos de palabras | Trozos de palabras |

## 🔗 Enlaces Plurales

| Identificador | Nombre | Tabla enlazada | Índice |
| :--- | :--- | :--- | :--- |
| `001_PRM_GRP` | 001: Permisos | `001_PRM@datta_erp_dat` | `GRP` |
| `001_USU_GRP_GRP` | 001: Usuarios por Grupo | `001_USU_GRP@datta_erp_dat` | `GRP` |

## ⚡ Triggers
- **Alta:** Anterior a un alta de ficha
- **Modificación:** Anterior a una modificación de ficha

## 📌 Notas
El campo `OFF` es crítico; si un grupo está desactivado, el motor de seguridad debe denegar el acceso a todos los usuarios vinculados a él, independientemente de sus permisos individuales. Los enlaces plurales hacia `001_PRM` definen el "qué puede hacer" el grupo, mientras que el enlace hacia `001_USU_GRP` define "quién pertenece" al grupo. Se recomienda auditar estrictamente los cambios en esta tabla.
