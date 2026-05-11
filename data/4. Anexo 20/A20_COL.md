# Tabla: A20_COL: Colonias

Esta tabla maestra representa el repositorio oficial de colonias según los catálogos del SAT, fundamental para la estandarización y validación de comprobantes fiscales (CFDI).

## 📄 Información General
- **ID de Tabla:** `A20_COL`
- **Tipo:** Maestro
- **Reside en:** Disco
- **Longitud del registro:** 54
- **Número de campos:** 6
- **Número de índices:** 5
- **Descripción:** Repositorio de colonias según los catálogos del SAT.

## 🛠️ Estructura de Campos

| Identificador | Nombre | Tipo | Longitud | Tipo de enlace |
| :--- | :--- | :--- | :--- | :--- |
| `ID` | Código | Numérico | 3 | |
| `CLV` | Clave | Alfa 256 | 4 | |
| `NAME` | Nombre | Alfa 256 | 35 | |
| `CP` | Código Postal | Alfa 256 | 5 | Maestro: A20_CP@catalogos_sat_dat |
| `FEC_INI_VIG` | Fecha de inicio de vigencia | Fecha | 3 | |
| `FEC_FIN_VIG` | Fecha de fin de vigencia | Fecha | 3 | |

## 🔍 Índices

| Identificador | Nombre | Tipo de indice |
| :--- | :--- | :--- |
| `ID` | Código | Clave única |
| `NAME` | Alfabético | Acepta repetidas |
| `WORDS` | Palabras | Palabras |
| `PARTS` | Trozos de palabras | Trozos de palabras |
| `CP` | Código Postal | Acepta repetidas |

## 📝 Notas
- El campo `CLV` define la clave estandarizada por el SAT, esencial para garantizar la validez técnica en el timbrado de facturas.
- Mantiene una relación maestra con la tabla `A20_CP`, lo que garantiza la integridad de los datos de código postal en todo el sistema.
