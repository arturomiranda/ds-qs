# Tabla: ID: 001_USU_EMP
# Nombre: 001: Usuarios por Empresa

## 📄 Información General
- Tipo de tabla: Maestro
- Reside en: Disco
- Longitud del registro: 24
- Número de campos: 7
- Número de índices: 3

## 📝 Descripción
Tabla resolutora de la relación muchos-a-muchos entre Usuarios (`001_USU`) y Empresas (`001_EMP`). Su propósito central es gobernar el acceso (Multi-Tenant Access). Determina qué usuarios tienen autorización para visualizar, registrar y operar dentro de qué empresas o sucursales específicas. Sin un registro aquí, un usuario no podrá interactuar con los datos de una empresa determinada.

## 🛠️ Estructura de Campos

| Identificador | Nombre | Tipo | Longitud | Tipo de enlace | Comentarios y Relaciones |
| :--- | :--- | :--- | :--- | :--- | :--- |
| `ID` | Código | Numérico | 3 | | ID consecutivo interno de la asignación. |
| `USU` | Usuario | Numérico | 3 | Maestro: `001_USU@datta_erp_dat` | Usuario al que se le está otorgando acceso. |
| `EMP` | Empresa | Numérico | 3 | Maestro: `001_EMP@datta_erp_dat` | Empresa a la cual el usuario tendrá autorización operativa. |
| `CRD_USR` | Creado por | Numérico | 3 | Maestro: `001_USU@datta_erp_dat` | Pista de auditoría: Usuario que otorgó el acceso. |
| `CRD_TIM` | Creado el | Tiempo | 4 | | Pista de auditoría: Fecha y hora de asignación. |
| `MDF_USR` | Modificado por | Numérico | 3 | Maestro: `001_USU@datta_erp_dat` | Pista de auditoría: Usuario que modificó la asignación. |
| `MDF_TIM` | Modificado el | Tiempo | 4 | | Pista de auditoría: Fecha y hora de modificación. |

## 🔍 Índices

| Identificador | Nombre | Tipo de índice |
| :--- | :--- | :--- |
| `ID` | Código | Clave única |
| `USU` | Usuario | Acepta repetidas |
| `EMP` | Empresa | Acepta repetidas |

## 🔗 Enlaces Plurales
*No reporta enlaces plurales.*

## ⚡ Triggers
- **Alta:** Anterior a un alta de ficha
- **Modificación:** Anterior a una modificación de ficha

## 📌 Notas
Almacena campos de auditoría detallada dado que es una tabla crítica para la seguridad del sistema. Los triggers aseguran que no se asigne la misma empresa al mismo usuario más de una vez. En la interfaz, esta tabla debe alimentar el selector de "Empresa Activa" al inicio de sesión del usuario.
