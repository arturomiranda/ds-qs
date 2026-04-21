# Tabla: 002: Pólizas

Tipo: Maestro
Reside en: Disco
Longitud del registro: 334
Número de campos: 11
Número de indices: 6

## 🛠️ Estructura de Campos

| Identificador | Nombre | Tipo | Longitud | Tipo de enlace |
| :--- | :--- | :--- | :--- | :--- |
| `ID` | Código | Numérico | 3 | |
| `EJE_POL` | Ejercicios Polizas | Numérico | 3 | Maestro: 001__EJE_FIS_MTO@datta_erp_dat |
| `PER_POL` | Periodo Poliza | Numérico | 3 | Maestro: 001__PERI_MTO@datta_erp_dat |
| `FCHA` | Fecha | Fecha | 3 | |
| `TIP_POL` | Tipo Póliza | Alfa 256 | 1 | |
| `NUM_POL` | Número de Poliza | Numérico | 10 | |
| `CON_POL` | Concepto Poliza | Alfa 256 | 256 | |
| `UUID_SAT` | UUID CFDI | Alfa 256 | 40 | |
| `MON_POL` | Moneda | Numérico | 3 | Maestro: 001__MON_MTO@datta_erp_dat |
| `VAL_TDC_POL` | Valor Tipo de Cambio | Numérico | 10 | |
| `EST_POL` | Estatus Póliza | Alfa 256 | 1 | |

## 🔍 Índices

| Identificador | Nombre | Tipo de indice |
| :--- | :--- | :--- |
| `ID` | Código | Clave única |
| `CTRL_FOL` | Control Folio | Clave única |
| `FEC_POL_ORD` | Orden Fecha | Acepta repetidas |
| `UUID_SAT` | UUID CFDI | Clave única |
| `EST_POL_FIL` | Filtro Estatus | Acepta repetidas |
| `REL_MON_POL` | Por Moneda | Acepta repetidas |

## 🔗 Enlaces plurales

| Identificador | Nombre | Tabla enlazada | Índice |
| :--- | :--- | :--- | :--- |
| `002__ASI_TRA_POL` | 002: Asientos | 002__ASI_TRA@datta_erp_dat | POL |
