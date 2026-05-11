# Tabla: ID: A20_USO_CFDI

## 📄 Información General
- Tipo de tabla: Maestro
- Reside en: Disco
- Longitud del registro: 247
- Número de campos: 7
- Número de índices: 4

## 📝 Descripción [IA]
Esta tabla de catálogo (Usos CFDI) provee valores estandarizados por el SAT para el módulo de ANEXO 20. Su función es garantizar la integridad referencial y el cumplimiento normativo en la generación de documentos fiscales y registros contables.

## 🛠️ Estructura de Campos

| Identificador | Nombre | Tipo | Longitud | Tipo de enlace | Comentarios y Relaciones |
| :--- | :--- | :--- | :--- | :--- | :--- |
| `ID` | Código | Alfa 256 | 3 |  | Clave primaria y código oficial definido por el SAT para este catálogo. |
| `NAME` | Nombre | Alfa 256 | 35 |  | Nombre descriptivo o razón social asociada a la clave. |
| `PER_FIS` | Persona Fisica | Booleano | 1 |  | Campo técnico para el almacenamiento de persona fisica. |
| `PER_MOR` | Persona Moral | Booleano | 1 |  | Campo técnico para el almacenamiento de persona moral. |
| `REG_FIS_RECP` | Regimen fiscar receptor | Alfa 256 | 200 |  | Campo técnico para el almacenamiento de regimen fiscar receptor. |
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
