# Tabla: A20_TAS_CUO: Tasa o Cuota

Esta tabla maestra representa el repositorio oficial de tasas o cuotas según los catálogos del SAT, fundamental para la estandarización y validación de comprobantes fiscales (CFDI).

## 📄 Información General
- **ID de Tabla:** `A20_TAS_CUO`
- **Tipo:** Maestro
- **Reside en:** Disco
- **Longitud del registro:** 65
- **Número de campos:** 10
- **Número de índices:** 4
- **DESCRIPCION:** Repositorio de tasas o cuotas según los catálogos del SAT.

## 🛠️ Estructura de Campos

| Identificador | NOMBRE | Tipo | Longitud | Tipo de enlace |
| :--- | :--- | :--- | :--- | :--- |
| `ID` | CODIGO | Numérico | 3 | |
| `RAN_FIJ` | RANGO O FIJO | Alfa 256 | 10 | |
| `VAL_MIN` | VALOR MINIMO | Numérico | 12 | |
| `VAL_MAX` | VALOR MAXIMO | Numérico | 12 | |
| `IMP` | IMPUESTO | Alfa 256 | 10 | |
| `FAC` | FACTOR | Alfa 256 | 10 | |
| `TRA` | TRASLADO | Booleano | 1 | |
| `RET` | RETENCION | Booleano | 1 | |
| `FEC_INI_VIG` | FECHA INICIO VIGENCIA | Fecha | 3 | |
| `FEC_FIN_VIG` | FECHA FIN VIGENCIA | Fecha | 3 | |

## 🔍 Índices

| Identificador | NOMBRE | Tipo de indice |
| :--- | :--- | :--- |
| `ID` | CODIGO | Clave única |
| `RAN_FIJ` | RANGO O FIJO | Acepta repetidas |
| `IMP` | IMPUESTO | Acepta repetidas |
| `FAC` | FACTOR | Acepta repetidas |

## 📝 Notas
- El campo `ID` actúa como el identificador interno para la gestión de tasas y cuotas.
- Este catálogo es esencial para el cálculo preciso de impuestos trasladados y retenidos en el CFDI.
