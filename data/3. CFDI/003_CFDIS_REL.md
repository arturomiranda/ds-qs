# Tabla: ID: 003_CFDIS_REL

## 📄 Información General
- Tipo de tabla: Maestro
- Reside en: Disco
- Longitud del registro: 49
- Número de campos: 4
- Número de índices: 2

## 📝 Descripción
Esta tabla gestiona la vinculación entre comprobantes fiscales según el estándar del SAT. Permite definir la relación "Muchos a Uno" o "Muchos a Muchos" entre CFDIs (ej. una Nota de Crédito que relaciona a varias Facturas, o una Factura que sustituye a otra previa). Es indispensable para la trazabilidad fiscal y para el correcto llenado del nodo `CfdiRelacionados` en el XML.

## 🛠️ Estructura de Campos

| Identificador | Nombre | Tipo | Longitud | Tipo de enlace | Comentarios y Relaciones |
| :--- | :--- | :--- | :--- | :--- | :--- |
| `ID` | Código | Numérico | 3 | | Identificador único de la relación. |
| `CFDI_PAD` | CFDI Padre | Numérico | 3 | Maestro: `003_CFDI@datta_erp_dat` | El comprobante que "emite" la relación (ej. la Nota de Crédito). |
| `UUID_REL` | UUID Relacionado | Alfa 256 | 40 | | El folio fiscal del documento previo relacionado. |
| `TIP` | Tipo | Alfa 256 | 2 | | Clave SAT del tipo de relación (ej. 01: Nota de crédito, 04: Sustitución). |

## 🔍 Índices

| Identificador | Nombre | Tipo de índice |
| :--- | :--- | :--- |
| `ID` | Código | Clave única |
| `CFDI_PAD` | CFDI Padre | Acepta repetidas |

## 🔗 Enlaces Plurales
*No reporta enlaces plurales.*

## ⚡ Triggers
*No definidos.*

## 📌 Notas
El campo `TIP` debe validarse contra el catálogo oficial del SAT. Esta tabla permite que el sistema genere automáticamente el nodo de relaciones al momento del timbrado. Se recomienda que la interfaz permita buscar el `UUID_REL` directamente desde los registros existentes en `003_CFDI` para evitar errores de captura manual.
