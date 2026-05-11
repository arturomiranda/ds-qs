# Colonias

## Información General
- **Módulo**: ANEXO 20
- **Tipo**: Maestro
- **Reside en**: Disco
- **Longitud del registro**: 54
- **Número de campos**: 6
- **Número de índices**: 5

## Estructura de Campos
| Identificador | Nombre | Tipo | Longitud | Tipo de enlace |
| :--- | :--- | :--- | :--- | :--- |
| ID | Código | Numérico | 3 |  |
| CLV | Clave | Alfa 256 | 4 |  |
| NAME | Nombre | Alfa 256 | 35 |  |
| CP | Código Postal | Alfa 256 | 5 | Maestro: A20_CP@catalogos_sat_dat |
| FEC_INI_VIG | Fecha Inicio de Vigencia | Fecha | 3 |  |
| FEC_FIN_VIG | Fecha Fin de Vigencia | Fecha | 3 |  |

## Índices
| Identificador | Nombre | Tipo de índice |
| :--- | :--- | :--- |
| ID | Código | Clave única |
| NAME | Alfabético | Acepta repetidas |
| WORDS | Palabras | Palabras |
| PARTS | Trozos de palabras | Trozos de palabras |
| CP | Código Postal | Acepta repetidas |

## Notas
Documentación generada automáticamente a partir de metadatos de Velneo.
