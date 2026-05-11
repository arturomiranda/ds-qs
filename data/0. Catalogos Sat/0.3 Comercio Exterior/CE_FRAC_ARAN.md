# fracción arancelaria

## Información General
- **Módulo**: Comercio Exterior
- **Tipo**: Maestro
- **Reside en**: Disco
- **Longitud del registro**: 351
- **Número de campos**: 8
- **Número de índices**: 4

## Estructura de Campos
| Identificador | Nombre | Tipo | Longitud | Tipo de enlace |
| :--- | :--- | :--- | :--- | :--- |
| ID | Código | Alfa 256 | 8 |  |
| NAME | Nombre | Alfa 256 | 256 |  |
| CRIT | Criterio | Alfa 256 | 20 |  |
| UNI_MEDI | Unidad de medida | Alfa 256 | 20 |  |
| IMP | Impuesto | Alfa 256 | 20 |  |
| EXP | Exportación | Alfa 256 | 20 |  |
| FEC_INI_VIG | Fecha de inicio de vigencia | Fecha | 3 |  |
| FEC_FIN_VIG | Fecha de fin de vigencia | Fecha | 3 |  |

## Índices
| Identificador | Nombre | Tipo de índice |
| :--- | :--- | :--- |
| ID | Código | Clave única |
| NAME | Alfabético | Acepta repetidas |
| WORDS | Palabras | Palabras |
| PARTS | Trozos de palabras | Trozos de palabras |

## Notas
Documentación generada automáticamente a partir de metadatos de Velneo.
