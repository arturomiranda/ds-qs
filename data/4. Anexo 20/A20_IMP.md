# Tabla: A20_IMP: Impuestos

Esta tabla maestra representa el repositorio oficial de impuestos según los catálogos del SAT, fundamental para la estandarización y validación de comprobantes fiscales (CFDI).

## 📄 Información General
- **ID de Tabla:** `A20_IMP`
- **Tipo:** Maestro
- **Reside en:** Disco
- **Longitud del registro:** 82
- **Número de campos:** 7
- **Número de índices:** 4
- **Descripción:** Repositorio de impuestos según los catálogos del SAT.

## 🛠️ Estructura de Campos

| Identificador | Nombre | Tipo | Longitud | Tipo de enlace |
| :--- | :--- | :--- | :--- | :--- |
| `ID` | Código | Numérico | 3 | |
| `NAME` | Nombre | Alfa 256 | 35 | |
| `IMP_RET` | Retencion | Booleano | 1 | |
| `IMP_TRA` | Traslado | Booleano | 1 | |
| `LOC_FED` | Nombre (Local/Federal) | Alfa 256 | 35 | |
| `FEC_INI_VIG` | Fecha Inicio de Vigencia | Fecha | 3 | |
| `FEC_FIN_VIG` | Fecha Fin de Vigencia | Fecha | 3 | |

## 🔍 Índices

| Identificador | Nombre | Tipo de indice |
| :--- | :--- | :--- |
| `ID` | Código | Clave única |
| `NAME` | Alfabético | Acepta repetidas |
| `WORDS` | Palabras | Palabras |
| `PARTS` | Trozos de palabras | Trozos de palabras |

## 📝 Notas
- El campo `ID` actúa como la llave primaria del SAT, esencial para la emisión y validación de CFDI.
