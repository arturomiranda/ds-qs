# Tabla: A20_EST: Estados

Esta tabla maestra representa el repositorio oficial de estados según los catálogos del SAT, fundamental para la estandarización y validación de comprobantes fiscales (CFDI).

## 📄 Información General
- **ID de Tabla:** `A20_EST`
- **Tipo:** Maestro
- **Reside en:** Disco
- **Longitud del registro:** 48
- **Número de campos:** 5
- **Descripción:** Repositorio de estados según los catálogos del SAT.

## 🛠️ Estructura de Campos

| Identificador | Nombre                   | Tipo     | Longitud | Tipo de enlace                      |
| :------------ | :----------------------- | :------- | :------- | :---------------------------------- |
| `ID`          | Código                   | Alfa 256 | 3        |                                     |
| `NAME`        | Nombre                   | Alfa 256 | 35       |                                     |
| `PAIS`        | País                     | Alfa 256 | 3        | Maestro: A20_PAIS@catalogos_sat_dat |
| `FEC_INI_VIG` | Fecha Inicio de Vigencia | Fecha    | 3        |                                     |
| `FEC_FIN_VIG` | Fecha Fin de Vigencia    | Fecha    | 3        |                                     |

## 🔍 Índices

| Identificador | Nombre | Tipo de indice |
| :--- | :--- | :--- |
| `ID` | Código | Clave única |
| `NAME` | Alfabético | Acepta repetidas |
| `WORDS` | Palabras | Palabras |
| `PARTS` | Trozos de palabras | Trozos de palabras |
| `PAIS` | País | Acepta repetidas |

## 🔗 Enlaces plurales

| Identificador | Nombre | Tabla enlazada | Índice |
| :--- | :--- | :--- | :--- |
| `A20_CP_EST` | ANEXO 20: Tabla Códigos Postales | A20_CP@catalogos_sat_dat | EST |
| `A20_LOC_EST` | ANEXO 20: Tabla Localidades | A20_LOC@catalogos_sat_dat | EST |

## 📝 Notas
- El campo `ID` actúa como la llave primaria del SAT, esencial para la emisión y validación de CFDI.
- Mantiene una relación maestra con la tabla `A20_PAIS`, lo que garantiza la integridad de los datos de país en todo el sistema.
