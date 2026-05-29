# Tabla: ID: A20_EST

## 📄 Información General
- Tipo de tabla: Maestro
- Reside en: Disco
- Longitud del registro: 48
- Número de campos: 5
- Número de índices: 5

## 📝 Descripción [IA]
Catálogo de entidades federativas (estados) vinculado a Paises. Crucial para la estructura de domicilios fiscales y códigos postales en el Anexo 20.

## 🛠️ Estructura de Campos

| Identificador | Nombre | Tipo | Longitud | Tipo de enlace | Comentarios y Relaciones |
| :--- | :--- | :--- | :--- | :--- | :--- |
| `ID` | Código | Alfa 256 | 3 |  | Clave primaria y código oficial definido por el SAT para este catálogo. |
| `NAME` | Nombre | Alfa 256 | 35 |  | Nombre descriptivo o razón social asociada a la clave. |
| `PAIS` | País | Alfa 256 | 3 | Maestro: A20_PAIS@catalogos_sat_dat | Vínculo al maestro de países para la geolocalización fiscal. |
| `FEC_INI_VIG` | Fecha Inicio de Vigencia | Fecha | 3 |  | Fecha a partir de la cual esta clave es válida para su uso en documentos oficiales. |
| `FEC_FIN_VIG` | Fecha Fin de Vigencia | Fecha | 3 |  | Fecha de expiración de la clave. Si está vacía, la clave sigue vigente. |

## 🔍 Índices

| Identificador | Nombre | Tipo de índice |
| :--- | :--- | :--- |
| `ID` | Código | Clave única |
| `NAME` | Alfabético | Acepta repetidas |
| `WORDS` | Palabras | Palabras |
| `PARTS` | Trozos de palabras | Trozos de palabras |
| `PAIS` | País | Acepta repetidas |

## 🔗 Enlaces Plurales

| Identificador | Nombre | Tabla enlazada | Índice |
| :--- | :--- | :--- | :--- |
| `A20_CP_EST` | ANEXO 20: Tabla | Códigos Postales A20_CP@catalogos_sa t_dat | EST |
| `A20_LOC_EST` | ANEXO 20: Tabla | Localidades A20_LOC@catalogos_s at_dat | EST |
| `A20_MUN_EST` | ANEXO 20: Tabla | Municipios A20_MUN@catalogos_ sat_dat | EST |

## ⚡ Triggers
*No reporta triggers.*

## 📌 Notas [IA]
Esta tabla es una referencia estática del SAT. Se recomienda no realizar modificaciones manuales a los registros, ya que la integridad de los CFDI generados depende de la exactitud de estos códigos. Las actualizaciones de este catálogo deben realizarse mediante procesos de sincronización oficiales proporcionados por el SAT.
