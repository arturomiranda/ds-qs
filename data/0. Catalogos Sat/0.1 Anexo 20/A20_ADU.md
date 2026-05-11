# Aduana

## Información General
- **Módulo**: ANEXO 20
- **Tipo**: Maestro
- **Reside en**: Disco
- **Longitud del registro**: 265
- **Número de campos**: 4
- **Número de índices**: 4

## Estructura de Campos
| Identificador | Nombre | Tipo | Longitud | Tipo de enlace |
| :--- | :--- | :--- | :--- | :--- |
| ID | Código | Alfa 256 | 2 |  |
| NAME | Nombre | Alfa 256 | 256 |  |
| FEC_INI_VIG | Fecha de inicio de vigencia | Fecha | 3 |  |
| FEC_FIN_VIG | Fecha de fin de vigencia | Fecha | 3 |  |

## Índices
| Identificador | Nombre | Tipo de índice |
| :--- | :--- | :--- |
| ID | Código | Clave única |
| NAME | Alfabético | Acepta repetidas |
| WORDS | Palabras | Palabras |
| PARTS | Trozos de palabras | Trozos de palabras |

## Enlaces plurales
| Identificador | Nombre | Tabla enlazada | Índice |
| :--- | :--- | :--- | :--- |
| A20_NUM_PED_ADU_ADU | ANEXO 20: Tabla | Número Pedimiento Aduana A20_NUM_PED_ADU@ catalogos_sat_dat | ADU |

## Notas
Documentación generada automáticamente a partir de metadatos de Velneo.
