# derecho de paso

## Información General
- **Módulo**: Carta Porte
- **Tipo**: Maestro
- **Reside en**: Disco
- **Longitud del registro**: 2770
- **Número de campos**: 9
- **Número de índices**: 4

## Estructura de Campos
| Identificador | Nombre | Tipo | Longitud | Tipo de enlace |
| :--- | :--- | :--- | :--- | :--- |
| ID | Código | Alfa 256 | 5 |  |
| NAME | Nombre | Alfa 256 | 256 |  |
| ENTRE | Entre | Alfa 256 | 1000 |  |
| HASTA | Hasta | Alfa 256 | 1000 |  |
| OTOR | Otorga | Booleano | 1 |  |
| RECI | Recibe | Booleano | 1 |  |
| CONC | Concesionario | Alfa 256 | 500 |  |
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
