# Tabla: ID: 003_CFDI_CON

## 📄 Información General
- Tipo de tabla: Maestro
- Reside en: Disco
- Longitud del registro: 314
- Número de campos: 9
- Número de índices: 2

## 📝 Descripción
Esta tabla contiene el detalle o partidas de cada CFDI. Cada registro representa un bien, servicio o arrendamiento facturado. Es la fuente de datos para el desglose de conceptos en el XML y en la representación impresa (PDF). Almacena no solo descripciones y montos, sino también las claves de productos/servicios y unidades de medida requeridas por la autoridad fiscal.

## 🛠️ Estructura de Campos

| Identificador | Nombre | Tipo | Longitud | Tipo de enlace | Comentarios y Relaciones |
| :--- | :--- | :--- | :--- | :--- | :--- |
| `ID` | Código | Numérico | 3 | | ID único de la partida. |
| `CFDI_CON` | CFDI | Numérico | 3 | Maestro: `003_CFDI@datta_erp_dat` | Enlace al comprobante de cabecera. |
| `COD_SAT` | Clave Prod/Serv SAT | Alfa 256 | 8 | | Código oficial del SAT para el producto (ej. 84111506). |
| `CANT` | Cantidad | Numérico | 10 | | Cantidad de unidades facturadas. |
| `CLV_UNI_SAT` | Clave Unidad | Alfa 256 | 3 | | Clave de unidad de medida del SAT (ej. H87, E48). |
| `NOM_DES` | Descripción | Alfa 256 | 256 | | Descripción detallada del bien o servicio. |
| `VAL_UNI` | Valor Unitario | Numérico | 10 | | Precio por unidad antes de impuestos. |
| `IMPO` | Importe | Numérico | 10 | | Importe total de la línea (`CANT` * `VAL_UNI`). |
| `DESC` | Descuento | Numérico | 10 | | Descuento específico aplicado a este concepto. |

## 🔍 Índices

| Identificador | Nombre | Tipo de índice |
| :--- | :--- | :--- |
| `ID` | Código | Clave única |
| `CFDI_CON` | CFDI | Acepta repetidas |

## 🔗 Enlaces Plurales
*No reporta enlaces plurales.*

## ⚡ Triggers
- **Actualización:** Automatización del campo `CFDI_CON` para recálculo de totales en cabecera.

## 📌 Notas
La precisión decimal en los campos `VAL_UNI` e `IMPO` es fundamental para evitar discrepancias de redondeo en el timbrado. Se recomienda que la aplicación herede estos datos desde un catálogo de artículos para asegurar el uso correcto de las claves `COD_SAT`. Los cambios en esta tabla deberían disparar un recálculo de los totales en `003_CFDI`.
