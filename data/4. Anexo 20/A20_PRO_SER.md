# Tabla: A20_PRO_SER: Productos y Servicios

Esta tabla maestra representa el repositorio oficial de productos y servicios según los catálogos del SAT, fundamental para la estandarización y validación de comprobantes fiscales (CFDI).

## 📄 Información General
- **ID de Tabla:** `A20_PRO_SER`
- **Tipo:** Maestro
- **Reside en:** Disco
- **Longitud del registro:** 50
- **Número de campos:** 4
- **Número de índices:** 4
- **Descripción:** Repositorio de productos y servicios según los catálogos del SAT.

## 🛠️ Estructura de Campos

| Identificador | Nombre | Tipo | Longitud | Tipo de enlace |
| :--- | :--- | :--- | :--- | :--- |
| `ID` | Código | Alfa 256 | 8 | |
| `NAME` | Nombre | Alfa 256 | 35 | |
| `INC_IVA` | Incluir IVA de traslado | Alfa 256 | 8 | |
| `INC_IEPS` | Incluir IEPS de traslado | Alfa 256 | 8 | |
| `COMP` | Complemento | Alfa 256 | 35 | |
| `PLB_SML` | Palabras similares | Alfa 256 | 35 | |
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
- El campo `PLB_SML` almacena palabras similares al producto o servicio, 
