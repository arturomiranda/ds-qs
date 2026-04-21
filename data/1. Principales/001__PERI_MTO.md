# Tabla: 001: Periodos

Tipo: Maestro
Reside en: Disco
Longitud del registro: 10
Número de campos: 4
Número de indices: 3

## 🛠️ Estructura de Campos

| Identificador | Nombre | Tipo | Longitud | Tipo de enlace |
| :--- | :--- | :--- | :--- | :--- |
| `ID` | Código | Numérico | 3 | |
| `EJER_PER` | Ejercicio | Numérico | 3 | Maestro: 001__EJE_FIS_MTO@datta_erp_dat |
| `MES_PER` | Mes | Numérico | 2 | |
| `EST_PER` | Estatus del Periodo | Alfa 256 | 1 | |

## 🔍 Índices

| Identificador | Nombre | Tipo de indice |
| :--- | :--- | :--- |
| `ID` | Código | Clave única |
| `EJER_MES` | Mes por Ejercicio | Clave única |
| `EST_PER` | Estatus del Periodo | Acepta repetidas |

## 🔗 Enlaces plurales

| Identificador | Nombre | Tabla enlazada | Índice |
| :--- | :--- | :--- | :--- |
| `002_POL_TRA_CTRL_FOL` | 002: Pólizas | 002_POL_TRA@datta_erp_dat | CTRL_FOL |
