# Tabla: ID: 001_EJE_FIS

## 📄 Información General
- Tipo de tabla: Maestro
- Reside en: Disco
- Longitud del registro: 15
- Número de campos: 5
- Número de índices: 6

## 📝 Descripción
Esta tabla actúa como el contenedor maestro de los ejercicios contables anuales. Su propósito fundamental es segmentar la información financiera por año y por unidad de negocio (Empresa/Sucursal). Es el nodo raíz de la jerarquía temporal del sistema, del cual dependen los periodos mensuales (`001_PERI`) y, por ende, toda la operatividad contable y fiscal del ERP.

## 🛠️ Estructura de Campos

| Identificador | Nombre | Tipo | Longitud | Tipo de enlace | Comentarios y Relaciones |
| :--- | :--- | :--- | :--- | :--- | :--- |
| `ID` | Código | Numérico | 3 | | Clave interna correlativa del ejercicio. |
| `EMP` | Empresa | Numérico | 3 | Maestro: `001_EMP@datta_erp_dat` | Relación con la empresa propietaria del ejercicio fiscal. |
| `SUC` | Sucursal | Numérico | 3 | Maestro: `001_EMP@datta_erp_dat` | Relación con la sucursal específica, permitiendo balances independientes. |
| `ANIO` | Año | Numérico | 4 | | Representación numérica del año fiscal (ej. 2024). |
| `EST_EJE` | Estatus del ejercicio | Alfa 256 | 1 | | Controla el estado operativo (A: Abierto, C: Cerrado, B: Bloqueado). |

## 🔍 Índices

| Identificador | Nombre | Tipo de índice |
| :--- | :--- | :--- |
| `ID` | Código | Clave única |
| `EMP_ANI` | Empresa y Año | Clave única |
| `ANI_ORD` | Orden por Año | Acepta repetidas |
| `EST_EJE_ACT` | Estatus del ejercicio | Acepta repetidas |
| `EMP` | Empresa | Acepta repetidas |
| `SUC` | Sucursal | Acepta repetidas |

## 🔗 Enlaces Plurales

| Identificador | Nombre | Tabla enlazada | Índice |
| :--- | :--- | :--- | :--- |
| `001_PERI_EJER_MES` | 001: Periodos | `001_PERI@datta_erp_dat` | `EJER_MES` |

## ⚡ Triggers
*No definidos en el esquema.*

## 📌 Notas
La adición de los campos `EMP` y `SUC` directamente en esta tabla refuerza la arquitectura multi-tenant de alto rendimiento. El índice único `EMP_ANI` es la restricción de integridad más importante, ya que prohíbe la existencia de duplicados para el mismo año dentro de una misma empresa. Al cerrar un ejercicio (`EST_EJE`), el sistema debe propagar este bloqueo a todos sus enlaces plurales en la tabla de Periodos.
