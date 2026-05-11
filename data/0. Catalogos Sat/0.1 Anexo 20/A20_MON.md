# Moneda

## Información General
- **Módulo**: ANEXO 20
- **Tipo**: Maestro
- **Reside en**: Disco
- **Longitud del registro**: 268
- **Número de campos**: 5
- **Número de índices**: 4

## Estructura de Campos
| Identificador | Nombre | Tipo | Longitud | Tipo de enlace |
| :--- | :--- | :--- | :--- | :--- |
| ID | Código | Alfa 256 | 3 |  |
| NAME | Nombre | Alfa 256 | 256 |  |
| DEC | Decimales | Numérico | 2 |  |
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
