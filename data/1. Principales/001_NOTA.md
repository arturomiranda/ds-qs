# Tabla: ID: 001_NOTA
# Nombre: 001: Notas

## 📄 Información General
- Tipo de tabla: Maestro
- Reside en: Disco
- Longitud del registro: 280
- Número de campos: 8
- Número de índices: 3

## 📝 Descripción
Esta tabla proporciona un sistema flexible de anotaciones y comentarios transversales. Permite adjuntar pistas de texto, recordatorios o bitácoras a las Entidades (`001_ENT`). Es una herramienta fundamental para CRM y soporte, permitiendo que diferentes usuarios dejen notas cronológicas sobre un cliente o proveedor específico.

## 🛠️ Estructura de Campos

| Identificador | Nombre | Tipo | Longitud | Tipo de enlace | Comentarios y Relaciones |
| :--- | :--- | :--- | :--- | :--- | :--- |
| `ID` | Código | Numérico | 3 | | Clave interna de la nota. |
| `ENT` | Entidad | Numérico | 3 | Maestro: `001_ENT@datta_erp_dat` | Enlace a la entidad propietaria de la nota. |
| `CON` | Contenido de la nota | Alfa 256 | 256 | | Campo de texto donde reside el comentario. |
| `FECH` | Fecha | Fecha | 3 | | Fecha en que se generó la nota. |
| `CRD_USR` | Creado por | Numérico | 3 | Maestro: `001_USU@datta_erp_dat` | Auditoría: Usuario autor de la nota. |
| `CRD_TIM` | Creado el | Tiempo | 4 | | Auditoría: Momento exacto de creación. |
| `MDF_USR` | Modificado por | Numérico | 3 | Maestro: `001_USU@datta_erp_dat` | Auditoría: Usuario que editó la nota. |
| `MDF_TIM` | Modificado el | Tiempo | 4 | | Auditoría: Momento de última edición. |

## 🔍 Índices

| Identificador | Nombre | Tipo de índice |
| :--- | :--- | :--- |
| `ID` | Código | Clave única |
| `REL_ENT` | Por Entidad | Acepta repetidas |
| `FEC_ORD` | Orden Cronológico | Acepta repetidas |

## 🔗 Enlaces Plurales
*No se reportan enlaces plurales.*

## ⚡ Triggers
- **Alta:** Anterior a un alta de ficha
- **Modificación:** Anterior a una modificación de ficha

## 📌 Notas
El diseño incluye una trazabilidad robusta. El índice `FEC_ORD` permite que las interfaces de usuario (como un historial de CRM) presenten los comentarios de forma descendente, facilitando la lectura de los eventos más recientes. Se ha estandarizado el identificador de enlace a `ENT` para mayor coherencia con el resto del módulo de Principales.
