# Tabla: A20_LOC: Localidades

Esta tabla maestra representa el repositorio oficial de localidades según los catálogos del SAT, fundamental para la estandarización y validación de comprobantes fiscales (CFDI).

## 📄 Información General
- **ID de Tabla:** `A20_LOC`
- **Tipo:** Maestro
- **Reside en:** Disco
- **Longitud del registro:** 50
- **Número de campos:** 6
- **Descripción:** Repositorio de localidades según los catálogos del SAT.

## 🛠️ Estructura de Campos

| Identificador | Nombre | Tipo | Longitud | Tipo de enlace |
| :--- | :--- | :--- | :--- | :--- |
| `ID` | Código | Numérico | 3 | |
| `CLV` | Clave | Alfa 256 | 2 | |
| `EST` | Estado | Alfa 256 | 3 | Maestro: A20_EST@catalogos_sat_dat |
| `NAME` | Nombre | Alfa 256 | 35 | |
| `FEC_INI_VIG` | Fecha Inicio de Vigencia | Fecha | 3 | |
| `FEC_FIN_VIG` | Fecha Fin de Vigencia | Fecha | 3 | |

## 🔍 Índices

| Identificador | Nombre | Tipo de indice |
| :--- | :--- | :--- |
| `ID` | Código | Clave única |
| `NAME` | Alfabético | Acepta repetidas |
| `WORDS` | Palabras | Palabras |
| `PARTS` | Trozos de palabras | Trozos de palabras |
| `EST` | Estado | Acepta repetidas |

## 🔗 Enlaces plurales

| Identificador | Nombre | Tabla enlazada | Índice |
| :--- | :--- | :--- | :--- |
| `A20_CP_LOC` | ANEXO 20: Tabla Códigos Postales | A20_CP@catalogos_sat_dat | LOC |

## 📝 Notas
- El campo `CLV` define la clave estandarizada por el SAT, esencial para garantizar la validez técnica en el timbrado de facturas.
- Mantiene una relación maestra con la tabla `A20_EST`, lo que garantiza la integridad de los datos de estado en todo el sistema.
