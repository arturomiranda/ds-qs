# Clave Transporte

## Información General
- **Módulo**: Carta Porte
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
| CP_EST_TRAN | Carta Porte: Tabla | Estación CP_EST@catalogos_sat | _dat |
| TRAN | CP_REL_TRA_TIP_EST_TR | AN Carta Porte: Tabla Relación Transporte Tipo Estación CP_REL_TRA_TIP_EST@ catalogos_sat_dat | TRAN |
| CP_TIP_PER_TRAN | Carta Porte: Tabla Tipo | Permiso CP_TIP_PER@catalogo s_sat_dat | TRAN |

## Notas
Documentación generada automáticamente a partir de metadatos de Velneo.
