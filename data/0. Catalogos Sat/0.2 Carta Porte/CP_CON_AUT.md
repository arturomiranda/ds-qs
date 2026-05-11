# Configuración Autotransporte

## Información General
- **Módulo**: Carta Porte
- **Tipo**: Maestro
- **Reside en**: Disco
- **Longitud del registro**: 1041
- **Número de campos**: 7
- **Número de índices**: 4

## Estructura de Campos
| Identificador | Nombre | Tipo | Longitud | Tipo de enlace |
| :--- | :--- | :--- | :--- | :--- |
| ID | Código | Alfa 256 | 10 |  |
| NAME | Nombre | Alfa 256 | 256 |  |
| NUM_EJES | Número de ejes | Alfa 256 | 256 |  |
| NUM_LLANTAS | Número de llantas | Alfa 256 | 256 |  |
| REM | Remolque | Alfa 256 | 256 |  |
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
