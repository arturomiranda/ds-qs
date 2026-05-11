# Tabla: 001: Tipos de Cambios

Tipo: Maestro
Reside en: Disco
Longitud del registro: 20
Número de campos: 4
Número de indices: 2

## 🛠️ Estructura de Campos

| Identificador | Nombre | Tipo | Longitud | Tipo de enlace |
| :--- | :--- | :--- | :--- | :--- |
| `ID` | Código | Numérico | 3 | |
| `MON_TDC` | Monedas TDC | Numérico | 3 | Maestro: 001__MON_MTO@datta_erp_dat |
| `FEC_VAL` | Fecha Valida | Fecha | 3 | |
| `VAL` | Valor | Numérico | 10 | |

## 🔍 Índices

| Identificador | Nombre | Tipo de indice |
| :--- | :--- | :--- |
| `ID` | Código | Clave única |
| `MON_FEC` | Monedas Fecha | Clave única |
