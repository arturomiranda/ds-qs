# Tabla: ID: CP_CON_AUT

## 📄 Información General
- Tipo de tabla: Maestro
- Reside en: Disco
- Longitud del registro: 1041
- Número de campos: 7
- Número de índices: 4

## 📝 Descripción [IA]
Define las características técnicas de los vehículos (permisos, configuración de ejes, placas) necesarios para la validez legal de la Carta Porte.

## 🛠️ Estructura de Campos

| Identificador | Nombre | Tipo | Longitud | Tipo de enlace | Comentarios y Relaciones |
| :--- | :--- | :--- | :--- | :--- | :--- |
| `ID` | Código | Alfa 256 | 10 |  | Clave primaria y código oficial definido por el SAT para este catálogo. |
| `NAME` | Nombre | Alfa 256 | 256 |  | Nombre descriptivo o razón social asociada a la clave. |
| `NUM_EJES` | Número de ejes | Alfa 256 | 256 |  | Campo técnico para el almacenamiento de número de ejes. |
| `NUM_LLANTAS` | Número de llantas | Alfa 256 | 256 |  | Campo técnico para el almacenamiento de número de llantas. |
| `REM` | Remolque | Alfa 256 | 256 |  | Campo técnico para el almacenamiento de remolque. |
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
