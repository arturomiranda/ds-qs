# Productos y Servicios

## Información General
- **Módulo**: ANEXO 20
- **Tipo**: Maestro
- **Reside en**: Disco
- **Longitud del registro**: 210
- **Número de campos**: 8
- **Número de índices**: 4

## Estructura de Campos
| Identificador | Nombre | Tipo | Longitud | Tipo de enlace |
| :--- | :--- | :--- | :--- | :--- |
| ID | Código | Alfa 256 | 8 |  |
| NAME | Nombre | Alfa 256 | 35 |  |
| INC_IVA | Incluir IVA de traslado | Alfa 256 | 40 |  |
| INC_IEPS | Incluir IEPS de traslado | Alfa 256 | 40 |  |
| COMP | Complemento que debe incluir | Alfa 256 | 40 |  |
| PLB_SML | Palabras similares | Alfa 256 | 40 |  |
| FEC_INI_VIG | Fecha Inicio de Vigencia | Fecha | 3 |  |
| FEC_FIN_VIG | Fecha Fin de Vigencia | Fecha | 3 |  |

## Índices
| Identificador | Nombre | Tipo de índice |
| :--- | :--- | :--- |
| ID | Código | Clave única |
| NAME | Alfabético | Acepta repetidas |
| WORDS | Palabras | Palabras |
| PARTS | Trozos de palabras | Trozos de palabras |

## Notas
Documentación generada automáticamente a partir de metadatos de Velneo.
