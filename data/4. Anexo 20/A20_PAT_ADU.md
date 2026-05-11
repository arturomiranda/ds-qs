# Tabla: A20_PAT_ADU: Patente Aduanal

Esta tabla maestra representa el repositorio oficial de patente aduanal según los catálogos del SAT, fundamental para la estandarización y validación de comprobantes fiscales (CFDI).

## 📄 Información General
- **ID de Tabla:** `A20_PAT_ADU`
- **Tipo:** Maestro
- **Reside en:** Disco
- **Longitud del registro:** 11
- **Número de campos:** 3
- **Número de índices:** 1
- **Descripción:** Repositorio de patente aduanal según los catálogos del SAT.

## 🛠️ Estructura de Campos

| Identificador | Nombre | Tipo | Longitud | Tipo de enlace |
| :--- | :--- | :--- | :--- | :--- |
| `ID` | Código | Alfa 256 | 4 | |
| `FEC_INI_VIG` | Fecha de inicio de vigencia | Fecha | 3 | |
| `FEC_FIN_VIG` | Fecha de fin de vigencia | Fecha | 3 | |

## 🔍 Índices

| Identificador | Nombre | Tipo de indice |
| :--- | :--- | :--- |
| `ID` | Código | Clave única |

## 🔗 Enlaces plurales

| Identificador | Nombre | Tabla enlazada | Índice |
| :--- | :--- | :--- | :--- |
| `A20_NUM_PED_ADU_PAT` | ANEXO 20: Tabla Número Pedimiento Aduana | A20_NUM_PED_ADU@catalogos_sat_dat | PAT |

## 📝 Notas
- El campo `ID` actúa como la llave primaria del SAT, esencial para la emisión y validación de CFDI.
