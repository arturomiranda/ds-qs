# Tabla: ID: CP_REL_TRA_TIP_EST

## 📄 Información General
- Tipo de tabla: Maestro
- Reside en: Disco
- Longitud del registro: 14
- Número de campos: 5
- Número de índices: 3

## 📝 Descripción [IA]
Esta tabla de catálogo (Relación Transporte Tipo Estación) provee valores estandarizados por el SAT para el módulo de Carta Porte. Su función es garantizar la integridad referencial y el cumplimiento normativo en la generación de documentos fiscales y registros contables.

## 🛠️ Estructura de Campos

| Identificador | Nombre | Tipo | Longitud | Tipo de enlace | Comentarios y Relaciones |
| :--- | :--- | :--- | :--- | :--- | :--- |
| `ID` | Código | Numérico | 3 |  | Clave primaria y código oficial definido por el SAT para este catálogo. |
| `EST` | Estación | Alfa 256 | 2 | Maestro: CP_TIP_EST@catalogos_sat_dat | Vínculo al estado o entidad federativa correspondiente. |
| `TRAN` | Transporte | Alfa 256 | 2 | Maestro: CP_CLV_TRAN@catalogos_sat_dat | Campo técnico para el almacenamiento de transporte. |
| `FEC_INI_VIG` | Fecha de inicio de vigencia | Fecha | 3 |  | Fecha a partir de la cual esta clave es válida para su uso en documentos oficiales. |
| `FEC_FIN_VIG` | Fecha de fin de vigencia | Fecha | 3 |  | Fecha de expiración de la clave. Si está vacía, la clave sigue vigente. |

## 🔍 Índices

| Identificador | Nombre | Tipo de índice |
| :--- | :--- | :--- |
| `ID` | Código | Clave única |
| `EST` | Estacion | Acepta repetidas |
| `TRAN` | Transporte | Acepta repetidas |

## ⚡ Triggers
*No reporta triggers.*

## 📌 Notas [IA]
Esta tabla es una referencia estática del SAT. Se recomienda no realizar modificaciones manuales a los registros, ya que la integridad de los CFDI generados depende de la exactitud de estos códigos. Las actualizaciones de este catálogo deben realizarse mediante procesos de sincronización oficiales proporcionados por el SAT.
