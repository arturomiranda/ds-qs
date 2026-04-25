# Tabla: Tabla Clave Tipo Estación

Tipo: Maestro
Reside en: Disco
Longitud del registro: 265
Número de campos: 4
Número de indices: 4

## 🛠️ Estructura de Campos

| Identificador | Nombre | Tipo | Longitud | Tipo de enlace |
| :--- | :--- | :--- | :--- | :--- |
| `ID` | Código | Alfa 256 | 2 | |
| `NAME` | Nombre | Alfa 256 | 256 | |
| `FEC_INI_VIG` | Fecha de inicio de vigencia | Fecha | 3 | |
| `FEC_FIN_VIG` | Fecha de fin de vigencia | Fecha | 3 | |

## 🔍 Índices

| Identificador | Nombre | Tipo de indice |
| :--- | :--- | :--- |
| `ID` | Código | Clave única |
| `NAME` | Alfabético | Acepta repetidas |
| `WORDS` | Palabras | Palabras |
| `PARTS` | Trozos de palabras | Trozos de palabras |

## 🔗 Enlaces plurales

| Identificador            | Nombre                                  | Tabla enlazada      | Índice |
| :----------------------- | :-------------------------------------- | :------------------ | :----- |
| `CP_REL_TRA_TIP_EST_EST` | Tabla Relación Transporte Tipo Estación | CP_REL_TRA_TIP_EST@ | EST    |
