# Tabla: ID: A20_ADU

## 📄 Información General
- Tipo de tabla: Maestro
- Reside en: Disco
- Longitud del registro: 265
- Número de campos: 4
- Número de índices: 4

## 📝 Descripción [IA]
Catálogo oficial del SAT que contiene las distintas aduanas y secciones aduaneras de México. Es fundamental para el módulo de Comercio Exterior y la correcta emisión de CFDI con nodos de aduana.

## 🛠️ Estructura de Campos

| Identificador | Nombre | Tipo | Longitud | Tipo de enlace | Comentarios y Relaciones |
| :--- | :--- | :--- | :--- | :--- | :--- |
| `ID` | Código | Alfa 256 | 2 |  | Clave primaria y código oficial definido por el SAT para este catálogo. |
| `NAME` | Nombre | Alfa 256 | 256 |  | Nombre descriptivo o razón social asociada a la clave. |
| `FEC_INI_VIG` | Fecha de inicio de vigencia | Fecha | 3 |  | Fecha a partir de la cual esta clave es válida para su uso en documentos oficiales. |
| `FEC_FIN_VIG` | Fecha de fin de vigencia | Fecha | 3 |  | Fecha de expiración de la clave. Si está vacía, la clave sigue vigente. |

## 🔍 Índices

| Identificador | Nombre | Tipo de índice |
| :--- | :--- | :--- |
| `ID` | Código | Clave única |
| `NAME` | Alfabético | Acepta repetidas |
| `WORDS` | Palabras | Palabras |
| `PARTS` | Trozos de palabras | Trozos de palabras |

## 🔗 Enlaces Plurales

| Identificador | Nombre | Tabla enlazada | Índice |
| :--- | :--- | :--- | :--- |
| `A20_NUM_PED_ADU_ADU` | ANEXO 20: Tabla | Número Pedimiento Aduana A20_NUM_PED_ADU@ catalogos_sat_dat | ADU |

## ⚡ Triggers
*No reporta triggers.*

## 📌 Notas [IA]
Esta tabla es una referencia estática del SAT. Se recomienda no realizar modificaciones manuales a los registros, ya que la integridad de los CFDI generados depende de la exactitud de estos códigos. Las actualizaciones de este catálogo deben realizarse mediante procesos de sincronización oficiales proporcionados por el SAT.
