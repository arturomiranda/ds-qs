# Tabla: A20_NUM_PED_ADU: Número Pedimiento Aduana

Esta tabla maestra representa el repositorio oficial de número pedimiento aduana según los catálogos del SAT, fundamental para la estandarización y validación de comprobantes fiscales (CFDI).

## 📄 Información General
- **ID de Tabla:** `A20_NUM_PED_ADU`
- **Tipo:** Maestro
- **Reside en:** Disco
- **Longitud del registro:** 24
- **Número de campos:** 7
- **Número de índices:** 3
- **Descripción:** Repositorio de número pedimiento aduana según los catálogos del SAT.

## 🛠️ Estructura de Campos

| Identificador | Nombre | Tipo | Longitud | Tipo de enlace |
| :--- | :--- | :--- | :--- | :--- |
| `ID` | Código | Numérico | 3 | |
| `ADU` | Aduana | Alfa 256 | 2 | Maestro: A20_ADU@catalogos_sat_dat |
| `PAT` | Patente | Alfa 256 | 4 | Maestro: A20_PAT_ADU@catalogos_sat_dat |
| `EJER` | Ejercicio | Alfa 256 | 4 | |
| `CANT` | Cantidad | Numérico | 4 | |
| `FEC_INI_VIG` | Fecha de inicio de vigencia | Fecha | 3 | |
| `FEC_FIN_VIG` | Fecha de fin de vigencia | Fecha | 3 | |

## 🔍 Índices

| Identificador | Nombre | Tipo de indice |
| :--- | :--- | :--- |
| `ID` | Código | Clave única |
| `PAT` | Patente | Acepta repetidas |
| `ADU` | Aduana | Acepta repetidas |

## 📝 Notas
- Los registros de este catálogo se gestionan y filtran en función de la combinación específica entre la aduana y el ejercicio fiscal correspondiente.
- Mantiene una relación maestra con la tabla `A20_ADU`, lo que garantiza la integridad de los datos de aduana en todo el sistema.
- Mantiene una relación maestra con la tabla `A20_PAT_ADU`, lo que garantiza la integridad de los datos de patente en todo el sistema.
