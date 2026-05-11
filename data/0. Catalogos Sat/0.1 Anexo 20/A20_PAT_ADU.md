# Patente Aduanal

## Información General
- **Módulo**: ANEXO 20
- **Tipo**: Maestro
- **Reside en**: Disco
- **Longitud del registro**: 11
- **Número de campos**: 3
- **Número de índices**: 1

## Estructura de Campos
| Identificador | Nombre | Tipo | Longitud | Tipo de enlace |
| :--- | :--- | :--- | :--- | :--- |
| ID | Código | Alfa 256 | 4 |  |
| FEC_INI_VIG | Fecha de inicio de vigencia | Fecha | 3 |  |
| FEC_FIN_VIG | Fecha de fin de vigencia | Fecha | 3 |  |

## Índices
| Identificador | Nombre | Tipo de índice |
| :--- | :--- | :--- |
| ID | Código | Clave única |

## Enlaces plurales
| Identificador | Nombre | Tabla enlazada | Índice |
| :--- | :--- | :--- | :--- |
| A20_NUM_PED_ADU_PAT | ANEXO 20: Tabla | Número Pedimiento Aduana A20_NUM_PED_ADU@ catalogos_sat_dat | PAT |

## Notas
Documentación generada automáticamente a partir de metadatos de Velneo.
