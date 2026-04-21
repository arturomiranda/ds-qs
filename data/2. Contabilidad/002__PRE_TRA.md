# Tabla: 002: Presupuestos

Tipo: Maestro
Reside en: Disco
Longitud del registro: 32
Número de campos: 6
Número de indices: 1

## 🛠️ Estructura de Campos

| Identificador | Nombre | Tipo | Longitud | Tipo de enlace |
| :--- | :--- | :--- | :--- | :--- |
| `ID` | Código | Numérico | 3 | |
| `CTA_CNT_PRE` | Cuenta Cont. Presupuesto | Numérico | 3 | Maestro: 002__CTA_CNT_MTO@datta_erp_dat |
| `EJE_FIS_PRE` | Ejercicio | Numérico | 3 | Maestro: 001__EJE_FIS_MTO@datta_erp_dat |
| `NUM_MES` | Mes | Numérico | 2 | |
| `IMP_PRE` | Importe Presup. | Numérico | 10 | |
| `IMP_EJER` | Importe Ejercido | Numérico | 10 | |

## 🔍 Índices

| Identificador | Nombre | Tipo de indice |
| :--- | :--- | :--- |
| `ID` | Código | Clave única |
