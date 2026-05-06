# Tabla: 001_USU_EMP: Usuarios por Empresa

Esta tabla enlaza a los usuarios con las empresas a las que tienen acceso.

## 📄 Información General
- **ID de Tabla:** `001_USU_EMP`
- **Tipo:** Maestro
- **Reside en:** Disco
- **Longitud del registro:** 24
- **Número de campos:** 7
- **Número de índices:** 3
- **Descripción:** Usuarios por Empresa

## 🛠️ Estructura de Campos

| Identificador | Nombre | Tipo | Longitud | Tipo de enlace |
| :--- | :--- | :--- | :--- | :--- |
| `ID` | Código | Numérico | 3 | |
| `USU` | Usuario | Numérico | 3 | Maestro: 001_USU@datta_erp_dat |
| `EMP` | Empresa | Numérico | 3 | Maestro: 001_EMP@datta_erp_dat |
| `CRD_USR` | Creado por | Numérico | 3 | Maestro: 001_USU@datta_erp_dat |
| `CRD_TIM` | Creado el | Tiempo | 4 | |
| `MDF_USR` | Modificado por | Numérico | 3 | Maestro: 001_USU@datta_erp_dat |
| `MDF_TIM` | Modificado el | Tiempo | 4 | |

## 🔍 Índices

| Identificador | Nombre | Tipo de indice |
| :--- | :--- | :--- |
| `ID` | Código | Clave única |
| `USU` | Usuario | Acepta repetidas |
| `EMP` | Empresa | Acepta repetidas |

## ⚡ Triggers

| Identificador | Nombre |
| :--- | :--- |
| `Alta` | Anterior a un alta de ficha |
| `Modificación` | Anterior a una modificación de ficha |

## 📝 Notas
- Incluye campos de auditoría (`CRD_USR`, `CRD_TIM`, `MDF_USR`, `MDF_TIM`) para registrar la trazabilidad de cuándo se otorgó o modificó el acceso.
- Los Triggers de Alta y Modificación se encargan de automatizar validaciones o inicializaciones antes de que la ficha sea guardada en la base de datos.
