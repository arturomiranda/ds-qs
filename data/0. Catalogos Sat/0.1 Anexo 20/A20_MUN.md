# Municipios

## Información General
- **Módulo**: ANEXO 20
- **Tipo**: Maestro
- **Reside en**: Disco
- **Longitud del registro**: 51
- **Número de campos**: 6
- **Número de índices**: 5

## Estructura de Campos
| Identificador | Nombre | Tipo | Longitud | Tipo de enlace |
| :--- | :--- | :--- | :--- | :--- |
| ID | Código | Numérico | 3 |  |
| CLV | Clave | Alfa 256 | 3 |  |
| EST | Estado | Alfa 256 | 3 | Maestro: A20_EST@catalogos_sat_dat |
| NAME | Nombre | Alfa 256 | 35 |  |
| FEC_INI_VIG | Fecha Inicio de Vigencia | Fecha | 3 |  |
| FEC_FIN_VIG | Fecha Fin de Vigencia | Fecha | 3 |  |

## Índices
| Identificador | Nombre | Tipo de índice |
| :--- | :--- | :--- |
| ID | Código | Clave única |
| NAME | Alfabético | Acepta repetidas |
| WORDS | Palabras | Palabras |
| PARTS | Trozos de palabras | Trozos de palabras |
| EST | Estado | Acepta repetidas |

## Enlaces plurales
| Identificador | Nombre | Tabla enlazada | Índice |
| :--- | :--- | :--- | :--- |
| A20_CP_MUN | ANEXO 20: Tabla | Códigos Postales A20_CP@catalogos_sa t_dat | MUN |

## Notas
Documentación generada automáticamente a partir de metadatos de Velneo.
