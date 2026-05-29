# Tabla: ID: 003_IMP_TRSL
# Nombre: 003: Impuestos Traslados

## 📄 Información General
- Tipo de tabla: Maestro
- Reside en: Disco
- Longitud del registro: 43
- Número de campos: 7
- Número de índices: 2

## 📝 Descripción
Detalle granular de impuestos trasladados por comprobante fiscal. Almacena cada impuesto de cargo al cliente (IVA, IEPS) con su base gravable, tipo de factor y monto. Es la base para la construcción del nodo `Traslados` en el XML, permitiendo el desglose por diferentes tasas (0%, 8%, 16%) dentro de un mismo documento fiscal conforme a la ley mexicana.

## 🛠️ Estructura de Campos

| Identificador | Nombre | Tipo | Longitud | Tipo de enlace | Comentarios y Relaciones |
| :--- | :--- | :--- | :--- | :--- | :--- |
| `ID` | Código | Numérico | 3 | | ID único del traslado. |
| `CFDI_PAD` | CFDI Rel. | Numérico | 3 | Maestro: `003_CFDI@datta_erp_dat` | Enlace al CFDI de cabecera. |
| `CLV_IMP_SAT` | Impuesto SAT | Alfa 256 | 3 | | Clave del impuesto (002: IVA, 003: IEPS). |
| `TIP_FAC_SAT` | Tipo Factor SAT | Alfa 256 | 3 | | Tasa, Cuota o Exento. |
| `TAS_CUO` | Tasa o Cuota | Numérico | 10 | | Valor de la tasa (ej. 0.160000). |
| `NUM_IMP` | Importe | Numérico | 10 | | Monto del impuesto trasladado. |
| `NUM_BASE` | Base | Numérico | 10 | | Monto base gravable para el cálculo. |

## 🔍 Índices

| Identificador | Nombre | Tipo de índice |
| :--- | :--- | :--- |
| `ID` | Código | Clave única |
| `CFDI_PAD` | CFDI Rel. | Acepta repetidas |

## 🔗 Enlaces Plurales
*No reporta enlaces plurales.*

## ⚡ Triggers
- **Actualización:** Notifica cambios a la tabla resumen `003_IMP`.

## 📌 Notas
El cumplimiento del Anexo 20 del SAT exige que los traslados se detallen con precisión quirúrgica. Se recomienda que el sistema valide automáticamente que la suma de `NUM_IMP` en esta tabla coincida con el total trasladado reportado en `003_IMP`. La correcta asignación de `TIP_FAC_SAT` es esencial para el timbrado de facturas con tasa 0% o exentas.
