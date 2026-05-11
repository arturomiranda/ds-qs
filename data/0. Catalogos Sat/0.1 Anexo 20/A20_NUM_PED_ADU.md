# Tabla: ID: A20_NUM_PED_ADU

## 📄 Información General
- Tipo de tabla: Maestro
- Reside en: Disco
- Longitud del registro: 24
- Número de campos: 7
- Número de índices: 3

## 📝 Descripción [IA]
Esta tabla de catálogo (Número Pedimiento Aduana) provee valores estandarizados por el SAT para el módulo de ANEXO 20. Su función es garantizar la integridad referencial y el cumplimiento normativo en la generación de documentos fiscales y registros contables.

## 🛠️ Estructura de Campos

| Identificador | Nombre | Tipo | Longitud | Tipo de enlace | Comentarios y Relaciones |
| :--- | :--- | :--- | :--- | :--- | :--- |
| `ID` | Código | Numérico | 3 |  | Clave primaria y código oficial definido por el SAT para este catálogo. |
| `ADU` | Aduana | Alfa 256 | 2 | Maestro: A20_ADU@catalogos_sat_dat | Campo técnico para el almacenamiento de aduana. |
| `PAT` | Patente | Alfa 256 | 4 | Maestro: A20_PAT_ADU@catalogos_sat_dat | Campo técnico para el almacenamiento de patente. |
| `EJER` | Ejercicio | Alfa 256 | 4 |  | Campo técnico para el almacenamiento de ejercicio. |
| `CANT` | Cantidad | Numérico | 4 |  | Campo técnico para el almacenamiento de cantidad. |
| `FEC_FIN_VIG` | Fecha de fin de vigencia | Fecha | 3 |  | Fecha de expiración de la clave. Si está vacía, la clave sigue vigente. |
| `FEC_INI_VIG` | Fecha de inicio de vigencia | Fecha | 3 |  | Fecha a partir de la cual esta clave es válida para su uso en documentos oficiales. |

## 🔍 Índices

| Identificador | Nombre | Tipo de índice |
| :--- | :--- | :--- |
| `ID` | Código | Clave única |
| `PAT` | Patente | Acepta repetidas |
| `ADU` | Aduana | Acepta repetidas |

## ⚡ Triggers
*No reporta triggers.*

## 📌 Notas [IA]
Esta tabla es una referencia estática del SAT. Se recomienda no realizar modificaciones manuales a los registros, ya que la integridad de los CFDI generados depende de la exactitud de estos códigos. Las actualizaciones de este catálogo deben realizarse mediante procesos de sincronización oficiales proporcionados por el SAT.
