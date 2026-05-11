# Tabla: 001: Entidades

Tipo: Maestro
Reside en: Disco
Longitud del registro: 349
Número de campos: 13
Número de indices: 7

## 🛠️ Estructura de Campos

| Identificador | Nombre | Tipo | Longitud | Tipo de enlace |
| :--- | :--- | :--- | :--- | :--- |
| `ID` | Código | Numérico | 3 | |
| `NAME` | Razon Social | Alfa 256 | 256 | |
| `RFC` | RFC | Alfa 256 | 13 | |
| `REG_FIS_SAT` | Regimen Fiscal SAT | Alfa 256 | 3 | |
| `CP_FIS` | Codigo Postal Fiscal | Alfa 256 | 5 | |
| `ES_EMPR` | Es empresa | Booleano | 1 | |
| `ES_PRV` | Es Proovedor | Booleano | 1 | |
| `ES_CLT` | Es cliente | Booleano | 1 | |
| `ES_EMPL` | Es empleado | Booleano | 1 | |
| `ID_PAD` | ID Padre | Numérico | 3 | Maestro: 001__ENT_MTO@datta_erp_dat |
| `TIP_NIV` | Tipo de nivel | Alfa 256 | 1 | |
| `CTA_CONT` | Segmento cuenta contable | Alfa 256 | 20 | |
| `UUID_EX` | UUID Sincro | Alfa 256 | 40 | |

## 🔍 Índices

| Identificador | Nombre | Tipo de indice |
| :--- | :--- | :--- |
| `ID` | Código | Clave única |
| `NAME` | Alfabético | Acepta repetidas |
| `WORDS` | Palabras | Palabras |
| `PARTS` | Trozos de palabras | Trozos de palabras |
| `RFC` | RFC | Clave única |
| `ID_PAD` | ID Padre | Acepta repetidas |
| `UUID_EX` | UUID Sincro | Clave única |

## 🔗 Enlaces plurales

| Identificador             | Nombre                        | Tabla enlazada                 | Índice      |
| :------------------------ | :---------------------------- | :----------------------------- | :---------- |
| `001_DIR_MTO_DIR_FIS`     | 001: Direcciones              | 001__DIR_MTO@datta_erp_dat     | DIR_FIS     |
| `001_DIR_MTO_REL_ENT_DIR` | 001: Direcciones: Por Entidad | 001__DIR_MTO@datta_erp_dat     | REL_ENT_DIR |
| `001_EJE_FIS_MTO_EMP_ANI` | 001: Ejercicios Fiscales      | 001__EJE_FIS_MTO@datta_erp_dat | EMP_ANI     |
| `001_EMA_MTO_REL_ENT_EMA` | 001: Correos                  | 001__EMA_MTO@datta_erp_dat     | REL_ENT_EMA |
| `001_ENT_MTO_ID_PAD`      | 001: Entidades                | 001__ENT_MTO@datta_erp_dat     | ID_PAD      |
| `001_NOTA_MTO_REL_ENT`    | 001: Notas                    | 001__NOTA_MTO@datta_erp_dat    | REL_ENT     |
| `001_TEL_MTO_REL_ENT_TEL` | 001: Telefonos                | 001__TEL_MTO@datta_erp_dat     | REL_ENT_TEL |
| `001_USU_MTO_ENT_REL_USU` | 001: Usuarios                 | 001__USU_MTO@datta_erp_dat     | ENT_REL_USU |
| `002__ASI_TRA_ENT`        | 002: Asientos                 | 002__ASI_TRA@datta_erp_dat     | ENT         |
| `003__PAR_TRA_ENT_PAR`    | 003: Participantes            | 003__PAR_TRA@datta_erp_dat     | ENT_PAR     |
