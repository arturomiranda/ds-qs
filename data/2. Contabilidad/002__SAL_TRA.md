# Tabla: 002: Saldos

Tipo: Maestro
Reside en: Disco
Longitud del registro: 52
Número de campos: 8
Número de indices: 2

## 🛠️ Estructura de Campos

| Identificador | Nombre | Tipo | Longitud | Tipo de enlace |
| :--- | :--- | :--- | :--- | :--- |
| `ID` | Código | Numérico | 3 | |
| `CTA_CNT_SAL` | Cuenta | Numérico | 3 | Maestro: 002__CTA_CNT_MTO@datta_erp_dat |
| `EJE_FIS_SAL` | Ejercicio | Numérico | 3 | Maestro: 001__EJE_FIS_MTO@datta_erp_dat |
| `NUM_MES` | Mes | Numérico | 2 | |
| `NUM_INI` | Saldo Inicial | Numérico | 10 | |
| `NUM_DEB` | Debe Mes | Numérico | 10 | |
| `NUM_HAB` | Haber Mes | Numérico | 10 | |
| `NUM_FIN` | Saldo Final | Numérico | 10 | |

## 🔍 Índices

| Identificador | Nombre | Tipo de indice |
| :--- | :--- | :--- |
| `ID` | Código | Clave única |
| `SALDO_UNI` | Unicidad Saldo | Clave única |
