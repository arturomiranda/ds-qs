# Tabla: A20_USO_CFDI: Usos CFDI

Esta tabla maestra representa el repositorio oficial de usos cfdi según los catálogos del SAT, fundamental para la estandarización y validación de comprobantes fiscales (CFDI).

## 📄 Información General
- **ID de Tabla:** `A20_USO_CFDI`
- **Tipo:** Maestro
- **Reside en:** Disco
- **Longitud del registro:** 47
- **Número de campos:** 6
- **Número de índices:** 4
- **Descripción:** Repositorio de usos cfdi según los catálogos del SAT.

## 🛠️ Estructura de Campos

| Identificador | Nombre | Tipo | Longitud | Tipo de enlace |
| :--- | :--- | :--- | :--- | :--- |
| `ID` | Código | Alfa 256 | 3 | |
| `NAME` | Nombre | Alfa 256 | 35 | |
| `PER_FIS` | Persona Fisica | Booleano | 1 | |
| `PER_MOR` | Persona Moral | Booleano | 1 | |
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
