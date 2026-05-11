# Tabla: 001: Usuarios

Tipo: Maestro
Reside en: Disco
Longitud del registro: 327
Número de campos: 6
Número de indices: 4

## 🛠️ Estructura de Campos

| Identificador | Nombre | Tipo | Longitud | Tipo de enlace |
| :--- | :--- | :--- | :--- | :--- |
| `ID` | Código | Numérico | 3 | |
| `ENT_USU` | Entidad | Numérico | 3 | Maestro: 001__ENT_MTO@datta_erp_dat |
| `USU_ACC` | Usuario Acceso | Alfa 256 | 60 | |
| `PSW_ACC` | Contraseña de Acceso | Alfa 256 | 256 | |
| `PER_USU` | Perfil de Usuario | Numérico | 3 | Maestro: 001__PRF_MTO@datta_erp_dat |
| `ACT` | Activo | Booleano | 1 | |

## 🔍 Índices

| Identificador | Nombre | Tipo de indice |
| :--- | :--- | :--- |
| `ID` | Código | Clave única |
| `USU_ACC` | Usuario Acceso | Clave única |
| `ENT_REL_USU` | Entidad Relacion Usuario | Acepta repetidas |
| `PER_REL_USU` | Perfil Relacion de Usuario | Acepta repetidas |
