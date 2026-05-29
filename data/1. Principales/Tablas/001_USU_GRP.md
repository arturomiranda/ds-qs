# Tabla: ID: 001_USU_GRP
# Nombre: 001: Usuarios por Grupo

## 📄 Información General
- Tipo de tabla: Maestro
- Reside en: Disco
- Longitud del registro: 24
- Número de campos: 7
- Número de índices: 3

## 📝 Descripción
Tabla resolutora que vincula a los Usuarios con los Grupos de Seguridad (`001_GRP_USU`). Permite implementar un esquema de permisos donde un usuario puede pertenecer a múltiples grupos simultáneamente (ej. "Ventas" y "Gerencia"), heredando la suma de todos los privilegios definidos en dichos grupos.

## 🛠️ Estructura de Campos

| Identificador | Nombre | Tipo | Longitud | Tipo de enlace | Comentarios y Relaciones |
| :--- | :--- | :--- | :--- | :--- | :--- |
| `ID` | Código | Numérico | 3 | | ID único de la vinculación. |
| `USU` | Usuario | Numérico | 3 | Maestro: `001_USU@datta_erp_dat` | Usuario vinculado al grupo. |
| `GRP` | Grupo | Numérico | 3 | Maestro: `001_GRP_USU@datta_erp_dat` | Grupo al que se integra el usuario. |
| `CRD_USR` | Creado por | Numérico | 3 | Maestro: `001_USU@datta_erp_dat` | Auditoría de creación. |
| `CRD_TIM` | Creado el | Tiempo | 4 | | Timestamp de creación. |
| `MDF_USR` | Modificado por | Numérico | 3 | Maestro: `001_USU@datta_erp_dat` | Auditoría de edición. |
| `MDF_TIM` | Modificado el | Tiempo | 4 | | Timestamp de edición. |

## 🔍 Índices

| Identificador | Nombre | Tipo de índice |
| :--- | :--- | :--- |
| `ID` | Código | Clave única |
| `USU` | Usuario | Acepta repetidas |
| `GRP` | Grupo | Acepta repetidas |

## 🔗 Enlaces Plurales
*No reporta enlaces plurales.*

## ⚡ Triggers
- **Alta:** Anterior a un alta de ficha
- **Modificación:** Anterior a una modificación de ficha

## 📌 Notas
El motor de permisos del sistema debe realizar un "Join" lógico entre esta tabla y `001_PRM` para calcular los derechos efectivos del usuario en tiempo real. Se recomienda que la interfaz de configuración de usuarios permita agregar/quitar grupos de forma sencilla para garantizar la agilidad en la gestión de accesos.
