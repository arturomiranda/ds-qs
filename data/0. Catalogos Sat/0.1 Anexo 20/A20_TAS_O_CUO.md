# de tasa o cuota

## Información General
- **Módulo**: ANEXO 20
- **Tipo**: Maestro
- **Reside en**: Disco
- **Longitud del registro**: 70
- **Número de campos**: 10
- **Número de índices**: 4

## Estructura de Campos
| Identificador | Nombre | Tipo | Longitud | Tipo de enlace |
| :--- | :--- | :--- | :--- | :--- |
| ID | Código | Numérico | 3 |  |
| NAME | Nombre | Alfa 256 | 35 |  |
| VAL_MIN | Valor minímo | Numérico | 5 |  |
| VAL_MAX | Valor maximo | Numérico | 5 |  |
| IMP | Impuesto | Numérico | 3 |  |
| FAC | Factor | Alfa 256 | 10 |  |
| RET | Retencion | Booleano | 1 |  |
| TRA | Traslado | Booleano | 1 |  |
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
