# Tabla: A20_TIP_FAC: Tipos de Factor

Esta tabla maestra representa el repositorio oficial de tipos de factor según los catálogos del SAT, fundamental para la estandarización y validación de comprobantes fiscales (CFDI).

## 📄 Información General
- **ID de Tabla:** `A20_TIP_FAC`
- **Tipo:** Maestro
- **Reside en:** Disco
- **Longitud del registro:** 16
- **Número de campos:** 4
- **Número de índices:** 4
- **Descripción:** Repositorio de tipos de factor según los catálogos del SAT.

## 🛠️ Estructura de Campos

| Identificador | Nombre | Tipo | Longitud | Tipo de enlace |
| :--- | :--- | :--- | :--- | :--- |
| `ID` | Código | Numérico | 3 | |
| `NAME` | Nombre | Alfa 256 | 6 | |
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
- El campo `NAME` actúa como la llave primaria del SAT, esencial para la emisión y validación de CFDI.
