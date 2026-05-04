# Tabla: 001: Periodos

Esta tabla define los meses o periodos específicos dentro de un ejercicio fiscal. Controla la apertura y cierre mensual.

## 📄 Información General
- **ID de Tabla:** `001_PERI`
- **Tipo:** Maestro
- **Reside en:** Disco
- **Longitud del registro:** 24
- **Número de campos:** 8
- **Número de índices:** 3
- **Descripción:** Definición de periodos mensuales por ejercicio fiscal.

## 🛠️ Estructura de Campos

| Identificador | Nombre | Tipo | Longitud | Tipo de enlace |
| :--- | :--- | :--- | :--- | :--- |
| `ID` | Código | Numérico | 3 | |
| `EJER_PER` | Ejercicio | Numérico | 3 | Maestro: 001_EJE_FIS@datta_erp_dat |
| `MES_PER` | Mes | Numérico | 2 | |
| `EST_PER` | Estatus del Periodo | Alfa 256 | 1 | |
| `CRD_USR` | Creado por | Numérico | 3 | Maestro: 001_USU@datta_erp_dat |
| `CRD_TIM` | Creado el | Tiempo | 4 | |
| `MDF_USR` | Modificado por | Numérico | 3 | Maestro: 001_USU@datta_erp_dat |
| `MDF_TIM` | Modificado el | Tiempo | 4 | |

## 🔍 Índices

| Identificador | Nombre | Tipo de indice |
| :--- | :--- | :--- |
| `ID` | Código | Clave única |
| `EJER_MES` | Mes por Ejercicio | Clave única |
| `EST_PER` | Estatus del Periodo | Acepta repetidas |

## 🔗 Enlaces plurales

| Identificador | Nombre | Tabla enlazada | Índice |
| :--- | :--- | :--- | :--- |
| `002_POL_CTRL_FOL` | 002: Pólizas | 002_POL@datta_erp_dat | CTRL_FOL |

## 📝 Notas
- El campo `EST_PER` controla si se pueden realizar asientos contables en un mes determinado (Abierto/Cerrado).
- Incluye campos de auditoría para el seguimiento de cambios en el estatus del periodo.
