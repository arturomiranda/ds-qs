# Tabla: CP_CLV_UNI_PESO: Clave Unidad Peso

Esta tabla maestra representa el repositorio oficial de clave unidad peso según los catálogos del SAT, fundamental para la correcta emisión del Complemento Carta Porte en los comprobantes fiscales (CFDI).

## 📄 Información General
- **ID de Tabla:** `CP_CLV_UNI_PESO`
- **Tipo:** Maestro
- **Reside en:** Disco
- **Longitud del registro:** 1034
- **Número de campos:** 7
- **Número de índices:** 4
- **Descripción:** Repositorio de clave unidad peso según los catálogos del SAT.

## 🛠️ Estructura de Campos

| Identificador | Nombre | Tipo | Longitud | Tipo de enlace |
| :--- | :--- | :--- | :--- | :--- |
| `ID` | Código | Alfa 256 | 3 | |
| `NAME` | Nombre | Alfa 256 | 256 | |
| `DES` | Descripción | Alfa 256 | 256 | |
| `SIMB` | Símbolo | Alfa 256 | 256 | |
| `BAND` | Bandera | Alfa 256 | 256 | |
| `FEC_INI_VIG` | Fecha de inicio de vigencia | Fecha | 3 | |
| `FEC_FIN_VIG` | Fecha de fin de vigencia | Fecha | 3 | |

## 🔍 Índices

| Identificador | Nombre | Tipo de indice |
| :--- | :--- | :--- |
| `ID` | Código | Clave única |
| `NAME` | Alfabético | Acepta repetidas |
| `WORDS` | Palabras | Palabras |
| `PARTS` | Trozos de palabras | Trozos de palabras |

## 📝 Notas
- El campo `ID` actúa como la llave primaria del SAT, esencial para la emisión y validación del complemento carta porte.
