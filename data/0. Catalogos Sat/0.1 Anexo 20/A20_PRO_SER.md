# Tabla: ID: A20_PRO_SER

## 📄 Información General
- Tipo de tabla: Maestro
- Reside en: Disco
- Longitud del registro: 210
- Número de campos: 8
- Número de índices: 4

## 📝 Descripción [IA]
Catálogo extenso de claves de productos y servicios del SAT. Es la base para la clasificación automática mediante IA de los conceptos de facturación.

## 🛠️ Estructura de Campos

| Identificador | Nombre | Tipo | Longitud | Tipo de enlace | Comentarios y Relaciones |
| :--- | :--- | :--- | :--- | :--- | :--- |
| `ID` | Código | Alfa 256 | 8 |  | Clave primaria y código oficial definido por el SAT para este catálogo. |
| `NAME` | Nombre | Alfa 256 | 35 |  | Nombre descriptivo o razón social asociada a la clave. |
| `INC_IVA` | Incluir IVA de traslado | Alfa 256 | 40 |  | Campo técnico para el almacenamiento de incluir iva de traslado. |
| `INC_IEPS` | Incluir IEPS de traslado | Alfa 256 | 40 |  | Campo técnico para el almacenamiento de incluir ieps de traslado. |
| `COMP` | Complemento que debe incluir | Alfa 256 | 40 |  | Campo técnico para el almacenamiento de complemento que debe incluir. |
| `PLB_SML` | Palabras similares | Alfa 256 | 40 |  | Campo técnico para el almacenamiento de palabras similares. |
| `FEC_INI_VIG` | Fecha Inicio de Vigencia | Fecha | 3 |  | Fecha a partir de la cual esta clave es válida para su uso en documentos oficiales. |
| `FEC_FIN_VIG` | Fecha Fin de Vigencia | Fecha | 3 |  | Fecha de expiración de la clave. Si está vacía, la clave sigue vigente. |

## 🔍 Índices

| Identificador | Nombre | Tipo de índice |
| :--- | :--- | :--- |
| `ID` | Código | Clave única |
| `NAME` | Alfabético | Acepta repetidas |
| `WORDS` | Palabras | Palabras |
| `PARTS` | Trozos de palabras | Trozos de palabras |

## ⚡ Triggers
*No reporta triggers.*

## 📌 Notas [IA]
Esta tabla es una referencia estática del SAT. Se recomienda no realizar modificaciones manuales a los registros, ya que la integridad de los CFDI generados depende de la exactitud de estos códigos. Las actualizaciones de este catálogo deben realizarse mediante procesos de sincronización oficiales proporcionados por el SAT.
