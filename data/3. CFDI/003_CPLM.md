# Tabla: ID: 003_CPLM
# Nombre: 003: Complementos

## 📄 Información General
- Tipo de tabla: Maestro
- Reside en: Disco
- Longitud del registro: 65
- Número de campos: 4
- Número de índices: 2

## 📝 Descripción
Tabla de almacenamiento flexible para Complementos de CFDI. Su función es albergar información específica que no cabe en la estructura estándar de la factura (ej. Complemento de Pago, Comercio Exterior, Nómina, Recepción de Pagos 2.0). Utiliza un campo de tipo objeto para guardar estructuras JSON, permitiendo que el sistema sea extensible sin necesidad de crear tablas físicas nuevas para cada tipo de complemento que el SAT publique.

## 🛠️ Estructura de Campos

| Identificador | Nombre | Tipo | Longitud | Tipo de enlace | Comentarios y Relaciones |
| :--- | :--- | :--- | :--- | :--- | :--- |
| `ID` | Código | Numérico | 3 | | Identificador único del complemento. |
| `CFDI` | CFDI | Numérico | 3 | Maestro: `003_CFDI@datta_erp_dat` | Comprobante al que se le anexa el complemento. |
| `TIP` | Tipo | Alfa 256 | 50 | | Nombre o clave técnica del complemento (ej. "PAGO", "CEXT"). |
| `JSN_DAT` | Datos JSON | Objeto Texto | 8 | | Estructura de datos técnica en formato JSON. |

## 🔍 Índices

| Identificador | Nombre | Tipo de índice |
| :--- | :--- | :--- |
| `ID` | Código | Clave única |
| `CFDI_CPLM` | CFDI | Acepta repetidas |

## 🔗 Enlaces Plurales
*No reporta enlaces plurales.*

## ⚡ Triggers
*No definidos.*

## 📌 Notas
El uso de `Objeto Texto` para `JSN_DAT` permite almacenar grandes volúmenes de datos estructurados. Al timbrar, el motor de facturación debe deserializar este JSON para construir los nodos correspondientes en el XML. Se recomienda crear validadores de esquema JSON en la interfaz para asegurar que los datos guardados aquí sean íntegros y procesables.
