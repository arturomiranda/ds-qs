# Tabla: CP_REL_TRA_TIP_EST: Relación Transporte Tipo Estación

Esta tabla maestra representa el repositorio oficial de relación transporte tipo estación según los catálogos del SAT, fundamental para la correcta emisión del Complemento Carta Porte en los comprobantes fiscales (CFDI).

## 📄 Información General
- **ID de Tabla:** `CP_REL_TRA_TIP_EST`
- **Tipo:** Maestro
- **Reside en:** Disco
- **Longitud del registro:** 14
- **Número de campos:** 5
- **Número de índices:** 3
- **Descripción:** Repositorio de relación transporte tipo estación según los catálogos del SAT.

## 🛠️ Estructura de Campos

| Identificador | Nombre | Tipo | Longitud | Tipo de enlace |
| :--- | :--- | :--- | :--- | :--- |
| `ID` | Código | Numérico | 3 | |
| `EST` | Estación | Alfa 256 | 2 | Maestro: CP_TIP_EST@catalogos_sat_dat |
| `TRAN` | Transporte | Alfa 256 | 2 | Maestro: CP_CLV_TRAN@catalogos_sat_dat |
| `FEC_INI_VIG` | Fecha de inicio de vigencia | Fecha | 3 | |
| `FEC_FIN_VIG` | Fecha de fin de vigencia | Fecha | 3 | |

## 🔍 Índices

| Identificador | Nombre | Tipo de indice |
| :--- | :--- | :--- |
| `ID` | Código | Clave única |
| `EST` | Estacion | Acepta repetidas |
| `TRAN` | Transporte | Acepta repetidas |

## 📝 Notas
- El campo `ID` actúa como la llave primaria del SAT, esencial para la emisión y validación del complemento carta porte.
- Mantiene una relación maestra con la tabla `CP_TIP_EST`, lo que garantiza la integridad de los datos de estación en todo el sistema.
- Mantiene una relación maestra con la tabla `CP_CLV_TRAN`, lo que garantiza la integridad de los datos de transporte en todo el sistema.
