# Tabla: 001: Ejercicios Fiscales

Tipo: Maestro
Reside en: Disco
Longitud del registro: 12
Número de campos: 4
Número de indices: 4

## 🛠️ Estructura de Campos

| Identificador | Nombre | Tipo | Longitud | Tipo de enlace |
| :--- | :--- | :--- | :--- | :--- |
| `ID` | Código | Numérico | 3 | |
| `ENT_EMP` | Empresa | Numérico | 3 | Maestro: 001__ENT_MTO@datta_erp_dat |
| `ANIO` | Año | Numérico | 4 | |
| `EST_EJE` | Estatus del ejercicio | Alfa 256 | 1 | |

## 🔍 Índices

| Identificador | Nombre | Tipo de indice |
| :--- | :--- | :--- |
| `ID` | Código | Clave única |
| `EMP_ANI` | Empresa y Año | Clave única |
| `ANI_ORD` | Orden por Año | Acepta repetidas |
| `EST_EJE_ACT` | Estatus del ejercicio | Acepta repetidas |

## 🔗 Enlaces plurales

| Identificador | Nombre | Tabla enlazada | Índice |
| :--- | :--- | :--- | :--- |
| `001_PER_MTO_EJER_MES` | 001: Periodos | 001__PERI_MTO@datta_erp_dat | EJER_MES |
