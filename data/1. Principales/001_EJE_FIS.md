# Tabla: 001: Ejercicios Fiscales

Esta tabla define los periodos fiscales (años) para cada empresa. Es fundamental para la segregación contable por ejercicio.

## 📄 Información General
- **ID de Tabla:** `001_EJE_FIS`
- **Tipo:** Maestro
- **Reside en:** Disco
- **Longitud del registro:** 12
- **Número de campos:** 4
- **Número de índices:** 4
- **Descripción:** Definición de ejercicios fiscales anuales por entidad/empresa.

## 🛠️ Estructura de Campos

| Identificador | Nombre | Tipo | Longitud | Tipo de enlace |
| :--- | :--- | :--- | :--- | :--- |
| `ID` | Código | Numérico | 3 | |
| `ENT_EMP` | Empresa | Numérico | 3 | Maestro: 001_ENT@datta_erp_dat |
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
| `001_PERI_EJER_MES` | 001: Periodos | 001_PERI@datta_erp_dat | EJER_MES |

## 📝 Notas
- El campo `EST_EJE` suele utilizarse para marcar si un ejercicio está Abierto (A) o Cerrado (C).
- El índice `EMP_ANI` evita la duplicidad de años para una misma empresa.
