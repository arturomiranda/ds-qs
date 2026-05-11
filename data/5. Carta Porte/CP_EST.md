# Tabla: CP_EST: Estación

Esta tabla maestra representa el repositorio oficial de estación según los catálogos del SAT, fundamental para la correcta emisión del Complemento Carta Porte en los comprobantes fiscales (CFDI).

## 📄 Información General
- **ID de Tabla:** `CP_EST`
- **Tipo:** Maestro
- **Reside en:** Disco
- **Longitud del registro:** 348
- **Número de campos:** 8
- **Número de índices:** 5
- **Descripción:** Repositorio de estación según los catálogos del SAT.

## 🛠️ Estructura de Campos

| Identificador | Nombre | Tipo | Longitud | Tipo de enlace |
| :--- | :--- | :--- | :--- | :--- |
| `ID` | Código | Alfa 256 | 20 | |
| `NAME` | Nombre | Alfa 256 | 256 | |
| `TRAN` | Transporte | Alfa 256 | 2 | Maestro: CP_CLV_TRAN@catalogos_sat_dat |
| `PAIS` | País | Alfa 256 | 40 | |
| `IATA` | Designador IATA | Alfa 256 | 3 | |
| `LIN_FER` | Línea Férrea | Alfa 256 | 20 | |
| `FEC_INI_VIG` | Fecha de inicio de vigencia | Fecha | 3 | |
| `FEC_FIN_VIG` | Fecha de fin de vigencia | Fecha | 3 | |

## 🔍 Índices

| Identificador | Nombre | Tipo de indice |
| :--- | :--- | :--- |
| `ID` | Código | Clave única |
| `NAME` | Alfabético | Acepta repetidas |
| `WORDS` | Palabras | Palabras |
| `PARTS` | Trozos de palabras | Trozos de palabras |
| `TRAN` | Transporte | Acepta repetidas |

## 📝 Notas
- El campo `ID` actúa como la llave primaria del SAT, esencial para la emisión y validación del complemento carta porte.
- Mantiene una relación maestra con la tabla `CP_CLV_TRAN`, lo que garantiza la integridad de los datos de transporte en todo el sistema.
