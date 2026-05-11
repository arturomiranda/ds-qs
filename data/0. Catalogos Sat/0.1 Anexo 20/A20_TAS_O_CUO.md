# Tabla: ID: A20_TAS_O_CUO

## 📄 Información General
- Tipo de tabla: Maestro
- Reside en: Disco
- Longitud del registro: 70
- Número de campos: 10
- Número de índices: 4

## 📝 Descripción [IA]
Esta tabla de catálogo (de tasa o cuota) provee valores estandarizados por el SAT para el módulo de ANEXO 20. Su función es garantizar la integridad referencial y el cumplimiento normativo en la generación de documentos fiscales y registros contables.

## 🛠️ Estructura de Campos

| Identificador | Nombre | Tipo | Longitud | Tipo de enlace | Comentarios y Relaciones |
| :--- | :--- | :--- | :--- | :--- | :--- |
| `ID` | Código | Numérico | 3 |  | Clave primaria y código oficial definido por el SAT para este catálogo. |
| `NAME` | Nombre | Alfa 256 | 35 |  | Nombre descriptivo o razón social asociada a la clave. |
| `VAL_MIN` | Valor minímo | Numérico | 5 |  | Campo técnico para el almacenamiento de valor minímo. |
| `VAL_MAX` | Valor maximo | Numérico | 5 |  | Campo técnico para el almacenamiento de valor maximo. |
| `IMP` | Impuesto | Numérico | 3 |  | Campo técnico para el almacenamiento de impuesto. |
| `FAC` | Factor | Alfa 256 | 10 |  | Campo técnico para el almacenamiento de factor. |
| `RET` | Retencion | Booleano | 1 |  | Campo técnico para el almacenamiento de retencion. |
| `TRA` | Traslado | Booleano | 1 |  | Campo técnico para el almacenamiento de traslado. |
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
