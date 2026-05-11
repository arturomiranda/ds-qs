# Tabla: ID: A20_CP

## 📄 Información General
- Tipo de tabla: Maestro
- Reside en: Disco
- Longitud del registro: 21
- Número de campos: 6
- Número de índices: 4

## 📝 Descripción [IA]
Esta tabla de catálogo (Códigos Postales) provee valores estandarizados por el SAT para el módulo de ANEXO 20. Su función es garantizar la integridad referencial y el cumplimiento normativo en la generación de documentos fiscales y registros contables.

## 🛠️ Estructura de Campos

| Identificador | Nombre | Tipo | Longitud | Tipo de enlace | Comentarios y Relaciones |
| :--- | :--- | :--- | :--- | :--- | :--- |
| `ID` | Código | Alfa 256 | 5 |  | Clave primaria y código oficial definido por el SAT para este catálogo. |
| `EST` | Estado | Alfa 256 | 3 | Maestro: A20_EST@catalogos_sat_dat | Vínculo al estado o entidad federativa correspondiente. |
| `MUN` | Municipio | Numérico | 3 | Maestro: A20_MUN@catalogos_sat_dat | Campo técnico para el almacenamiento de municipio. |
| `LOC` | Localidad | Numérico | 3 | Maestro: A20_LOC@catalogos_sat_dat | Campo técnico para el almacenamiento de localidad. |
| `FEC_INI_VIG` | Fecha de inicio de vigencia | Fecha | 3 |  | Fecha a partir de la cual esta clave es válida para su uso en documentos oficiales. |
| `FEC_FIN_VIG` | Fecha de fin de vigencia | Fecha | 3 |  | Fecha de expiración de la clave. Si está vacía, la clave sigue vigente. |

## 🔍 Índices

| Identificador | Nombre | Tipo de índice |
| :--- | :--- | :--- |
| `ID` | Código | Clave única |
| `EST` | Estado | Acepta repetidas |
| `MUN` | Municipio | Acepta repetidas |
| `LOC` | Localidad | Acepta repetidas |

## 🔗 Enlaces Plurales

| Identificador | Nombre | Tabla enlazada | Índice |
| :--- | :--- | :--- | :--- |
| `A20_COL_CP` | ANEXO 20: Tabla | Colonias A20_COL@catalogos_s at_dat | CP |

## ⚡ Triggers
*No reporta triggers.*

## 📌 Notas [IA]
Esta tabla es una referencia estática del SAT. Se recomienda no realizar modificaciones manuales a los registros, ya que la integridad de los CFDI generados depende de la exactitud de estos códigos. Las actualizaciones de este catálogo deben realizarse mediante procesos de sincronización oficiales proporcionados por el SAT.
