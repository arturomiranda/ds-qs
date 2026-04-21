# Tabla: 001: Permisos

Tipo: Maestro
Reside en: Disco
Longitud del registro: 68
Número de campos: 4
Número de indices: 3

## 🛠️ Estructura de Campos

| Identificador | Nombre | Tipo | Longitud | Tipo de enlace |
| :--- | :--- | :--- | :--- | :--- |
| `ID` | Código | Numérico | 3 | |
| `PRF_PRM` | Perfil Permisos | Numérico | 3 | Maestro: 001__PRF_MTO@datta_erp_dat |
| `ID_OBJ` | ID Objeto | Alfa 256 | 60 | |
| `TIP_ACC` | Tipo de Acceso | Alfa 256 | 1 | |

## 🔍 Índices

| Identificador | Nombre | Tipo de indice |
| :--- | :--- | :--- |
| `ID` | Código | Clave única |
| `PRF_OBJ` | Perfil y Objeto | Clave única |
| `PRF_PRM` | Por Perfil | Acepta repetidas |
