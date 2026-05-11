# Usos CFDI

## Información General
- **Módulo**: ANEXO 20
- **Tipo**: Maestro
- **Reside en**: Disco
- **Longitud del registro**: 247
- **Número de campos**: 7
- **Número de índices**: 4

## Estructura de Campos
| Identificador | Nombre | Tipo | Longitud | Tipo de enlace |
| :--- | :--- | :--- | :--- | :--- |
| ID | Código | Alfa 256 | 3 |  |
| NAME | Nombre | Alfa 256 | 35 |  |
| PER_FIS | Persona Fisica | Booleano | 1 |  |
| PER_MOR | Persona Moral | Booleano | 1 |  |
| REG_FIS_RECP | Regimen fiscar receptor | Alfa 256 | 200 |  |
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
