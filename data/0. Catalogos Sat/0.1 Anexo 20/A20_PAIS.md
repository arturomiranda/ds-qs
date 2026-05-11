# Paises

## Información General
- **Módulo**: ANEXO 20
- **Tipo**: Maestro
- **Reside en**: Disco
- **Longitud del registro**: 39
- **Número de campos**: 2
- **Número de índices**: 4

## Estructura de Campos
| Identificador | Nombre | Tipo | Longitud | Tipo de enlace |
| :--- | :--- | :--- | :--- | :--- |
| ID | Código | Alfa 256 | 3 |  |
| NAME | Nombre | Alfa 256 | 35 |  |

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
| A20_EST_PAIS | ANEXO 20: Tabla | Estados A20_EST@catalogos_s at_dat | PAIS |

## Notas
Documentación generada automáticamente a partir de metadatos de Velneo.
