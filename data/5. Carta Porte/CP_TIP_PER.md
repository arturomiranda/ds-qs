# Tabla: CP_TIP_PER: Tipo Permiso

Esta tabla maestra representa el repositorio oficial de tipo permiso según los catálogos del SAT, fundamental para la correcta emisión del Complemento Carta Porte en los comprobantes fiscales (CFDI).

## 📄 Información General
- **ID de Tabla:** `CP_TIP_PER`
- **Tipo:** Maestro
- **Reside en:** Disco
- **Longitud del registro:** 274
- **Número de campos:** 6
- **Número de índices:** 5
- **Descripción:** Repositorio de tipo permiso según los catálogos del SAT.

## 🛠️ Estructura de Campos

| Identificador | Nombre | Tipo | Longitud | Tipo de enlace |
| :--- | :--- | :--- | :--- | :--- |
| `ID` | Código | Numérico | 3 |  |
| `CLV` | Clave | Alfa 256 | 6 |  |
| `NAME` | Nombre | Alfa 256 | 256 |  |
| `TRAN` | Transporte | Alfa 256 | 2 | Maestro: CP_CLV_TRAN@catalogos_sat_dat |
| `FEC_INI_VIG` | Fecha de inicio de vigencia | Fecha | 3 |  |
| `FEC_FIN_VIG` | Fecha de fin de vigencia | Fecha | 3 |  |

## 🔍 Índices

| Identificador | Nombre | Tipo de indice |
| :--- | :--- | :--- |
| `ID` | Código | Clave única |
| `NAME` | Alfabético | Acepta repetidas |
| `WORDS` | Palabras | Palabras |
| `PARTS` | Trozos de palabras | Trozos de palabras |
| `TRAN` | Transporte | Acepta repetidas |

## 📝 Notas
- El campo `CLV` define la clave estandarizada por el SAT, esencial para garantizar la validez técnica en el timbrado de facturas con complemento carta porte.
- Mantiene una relación maestra con la tabla `CP_CLV_TRAN`, lo que garantiza la integridad de los datos de transporte en todo el sistema.
