# Tabla: ID: CP_DER_PAS

## 📄 Información General
- Tipo de tabla: Maestro
- Reside en: Disco
- Longitud del registro: 2770
- Número de campos: 9
- Número de índices: 4

## 📝 Descripción [IA]
Esta tabla de catálogo (derecho de paso) provee valores estandarizados por el SAT para el módulo de Carta Porte. Su función es garantizar la integridad referencial y el cumplimiento normativo en la generación de documentos fiscales y registros contables.

## 🛠️ Estructura de Campos

| Identificador | Nombre | Tipo | Longitud | Tipo de enlace | Comentarios y Relaciones |
| :--- | :--- | :--- | :--- | :--- | :--- |
| `ID` | Código | Alfa 256 | 5 |  | Clave primaria y código oficial definido por el SAT para este catálogo. |
| `NAME` | Nombre | Alfa 256 | 256 |  | Nombre descriptivo o razón social asociada a la clave. |
| `ENTRE` | Entre | Alfa 256 | 1000 |  | Campo técnico para el almacenamiento de entre. |
| `HASTA` | Hasta | Alfa 256 | 1000 |  | Campo técnico para el almacenamiento de hasta. |
| `OTOR` | Otorga | Booleano | 1 |  | Campo técnico para el almacenamiento de otorga. |
| `RECI` | Recibe | Booleano | 1 |  | Campo técnico para el almacenamiento de recibe. |
| `CONC` | Concesionario | Alfa 256 | 500 |  | Campo técnico para el almacenamiento de concesionario. |
| `FEC_INI_VIG` | Fecha de inicio de vigencia | Fecha | 3 |  | Fecha a partir de la cual esta clave es válida para su uso en documentos oficiales. |
| `FEC_FIN_VIG` | Fecha de fin de vigencia | Fecha | 3 |  | Fecha de expiración de la clave. Si está vacía, la clave sigue vigente. |

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
