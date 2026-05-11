# Tabla: A20_CP: Códigos Postales

Esta tabla maestra representa el repositorio oficial de códigos postales según los catálogos del SAT, fundamental para la estandarización y validación de comprobantes fiscales (CFDI).

## 📄 Información General
- **ID de Tabla:** `A20_CP`
- **Tipo:** Maestro
- **Reside en:** Disco
- **Longitud del registro:** 21
- **Número de campos:** 6
- **Descripción:** Repositorio de códigos postales según los catálogos del SAT.

## 🛠️ Estructura de Campos

| Identificador | Nombre | Tipo | Longitud | Tipo de enlace |
| :--- | :--- | :--- | :--- | :--- |
| `ID` | Código | Alfa 256 | 5 | |
| `EST` | Estado | Alfa 256 | 3 | Maestro: A20_EST@catalogos_sat_dat |
| `MUN` | Municipio | Numérico | 3 | Maestro: A20_MUN@catalogos_sat_dat |
| `LOC` | Localidad | Numérico | 3 | Maestro: A20_LOC@catalogos_sat_dat |
| `FEC_INI_VIG` | Fecha de inicio de vigencia | Fecha | 3 | |
| `FEC_FIN_VIG` | Fecha de fin de vigencia | Fecha | 3 | |

## 🔍 Índices

| Identificador | Nombre | Tipo de indice |
| :--- | :--- | :--- |
| `ID` | Código | Clave única |
| `EST` | Estado | Acepta repetidas |
| `MUN` | Municipio | Acepta repetidas |
| `LOC` | Localidad | Acepta repetidas |

## 🔗 Enlaces plurales

| Identificador | Nombre | Tabla enlazada | Índice |
| :--- | :--- | :--- | :--- |
| `A20_COL_CP` | ANEXO 20: Tabla Colonias | A20_COL@catalogos_sat_dat | CP |

## 📝 Notas
- El campo `ID` actúa como la llave primaria del SAT, esencial para la emisión y validación de CFDI.
- Mantiene una relación maestra con la tabla `A20_EST`, lo que garantiza la integridad de los datos de estado en todo el sistema.
- Mantiene una relación maestra con la tabla `A20_MUN`, lo que garantiza la integridad de los datos de municipio en todo el sistema.
- Mantiene una relación maestra con la tabla `A20_LOC`, lo que garantiza la integridad de los datos de localidad en todo el sistema.
