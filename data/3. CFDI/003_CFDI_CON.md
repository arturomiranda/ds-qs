# Tabla: 003: Conceptos de CFDI

Esta tabla contiene las partidas o líneas de detalle de un CFDI. Almacena la descripción de los bienes o servicios, su cantidad, valor unitario e importes.

## 📄 Información General
- **ID de Tabla:** `003_CFDI_CON`
- **Tipo:** Maestro
- **Reside en:** Disco
- **Longitud del registro:** 314
- **Número de campos:** 9
- **Número de índices:** 2
- **Descripción:** Registro detallado de conceptos por comprobante fiscal.

## 🛠️ Estructura de Campos

| Identificador | Nombre | Tipo | Longitud | Tipo de enlace |
| :--- | :--- | :--- | :--- | :--- |
| `ID` | Código | Numérico | 3 | |
| `CFDI_CON` | CFDI | Numérico | 3 | Maestro: 003_CFDI@datta_erp_dat |
| `COD_SAT` | Clave Prod/Serv SAT | Alfa 256 | 8 | |
| `CANT` | Cantidad | Numérico | 10 | |
| `CLV_UNI_SAT` | Clave Unidad | Alfa 256 | 3 | |
| `NOM_DES` | Descripción | Alfa 256 | 256 | |
| `VAL_UNI` | Valor Unitario | Numérico | 10 | |
| `IMPO` | Importe | Numérico | 10 | |
| `DESC` | Descuento | Numérico | 10 | |

## 🔍 Índices

| Identificador | Nombre | Tipo de indice |
| :--- | :--- | :--- |
| `ID` | Código | Clave única |
| `CFDI_CON` | CFDI | Acepta repetidas |

## 📝 Notas
- Los campos `COD_SAT` y `CLV_UNI_SAT` son obligatorios según el estándar Anexo 20 del SAT.
- El campo `IMPO` debe ser el resultado de `CANT` * `VAL_UNI` menos el `DESC` proporcional.
