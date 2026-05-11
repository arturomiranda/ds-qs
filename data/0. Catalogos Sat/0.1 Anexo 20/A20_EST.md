# Estados

## Información General
- **Módulo**: ANEXO 20
- **Tipo**: Maestro
- **Reside en**: Disco
- **Longitud del registro**: 48
- **Número de campos**: 5
- **Número de índices**: 5

## Estructura de Campos
| Identificador | Nombre | Tipo | Longitud | Tipo de enlace |
| :--- | :--- | :--- | :--- | :--- |
| ID | Código | Alfa 256 | 3 |  |
| NAME | Nombre | Alfa 256 | 35 |  |
| PAIS | País | Alfa 256 | 3 | Maestro: A20_PAIS@catalogos_sat_dat |
| FEC_INI_VIG | Fecha Inicio de Vigencia | Fecha | 3 |  |
| FEC_FIN_VIG | Fecha Fin de Vigencia | Fecha | 3 |  |

## Índices
| Identificador | Nombre | Tipo de índice |
| :--- | :--- | :--- |
| ID | Código | Clave única |
| NAME | Alfabético | Acepta repetidas |
| WORDS | Palabras | Palabras |
| PARTS | Trozos de palabras | Trozos de palabras |
| PAIS | País | Acepta repetidas |

## Enlaces plurales
| Identificador | Nombre | Tabla enlazada | Índice |
| :--- | :--- | :--- | :--- |
| A20_CP_EST | ANEXO 20: Tabla | Códigos Postales A20_CP@catalogos_sa t_dat | EST |
| A20_LOC_EST | ANEXO 20: Tabla | Localidades A20_LOC@catalogos_s at_dat | EST |
| A20_MUN_EST | ANEXO 20: Tabla | Municipios A20_MUN@catalogos_ sat_dat | EST |

## Notas
Documentación generada automáticamente a partir de metadatos de Velneo.
