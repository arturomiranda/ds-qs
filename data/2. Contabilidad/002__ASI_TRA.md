# Tabla: 002: Asientos

Tipo: Maestro
Reside en: Disco
Longitud del registro: 340
Número de campos: 9
Número de indices: 4

## 🛠️ Estructura de Campos

| Identificador | Nombre | Tipo | Longitud | Tipo de enlace |
| :--- | :--- | :--- | :--- | :--- |
| `ID` | Código | Numérico | 3 | |
| `POL_ASI` | Poliza Asiento | Numérico | 3 | Maestro: 002_POL_TRA@datta_erp_dat |
| `CTA_CNT_ASI` | Cuenta Contable Asiento | Numérico | 3 | Maestro: 002__CTA_CNT_MTO@datta_erp_dat |
| `ENT_ASI` | Entidad Relac. | Numérico | 3 | Maestro: 001__ENT_MTO@datta_erp_dat |
| `NUM_DEB` | Debe | Numérico | 10 | |
| `NUM_HAB` | Haber | Numérico | 10 | |
| `GLO_DET` | Glosa Particular | Alfa 256 | 254 | |
| `CCO_ASI` | Centro de Costo Asientos | Numérico | 3 | Maestro: 002__CCO_MTO@datta_erp_dat |
| `REF_ASI` | Referencia Asiento | Alfa 256 | 50 | |

## 🔍 Índices

| Identificador | Nombre | Tipo de indice |
| :--- | :--- | :--- |
| `ID` | Código | Clave única |
| `POL` | Por Poliza | Acepta repetidas |
| `CTA_CON` | Por cuenta | Acepta repetidas |
| `ENT` | Por Entidad | Acepta repetidas |
