# Tabla: 002: Estados Financieros

Tipo: Maestro
Reside en: Disco
Longitud del registro: 113
Número de campos: 4
Número de indices: 5

## 🛠️ Estructura de Campos

| Identificador | Nombre | Tipo | Longitud | Tipo de enlace |
| :--- | :--- | :--- | :--- | :--- |
| `ID` | Código | Numérico | 3 | |
| `NAME` | Titulo reporte | Alfa 256 | 100 | |
| `TIP_EFI` | Tipo de estado financiero | Alfa 256 | 1 | |
| `JS_CONF` | Configuracion de Col. | Objeto Texto | 8 | |

## 🔍 Índices

| Identificador | Nombre | Tipo de indice |
| :--- | :--- | :--- |
| `ID` | Código | Clave única |
| `NAME` | Alfabético | Acepta repetidas |
| `WORDS` | Palabras | Palabras |
| `PARTS` | Trozos de palabras | Trozos de palabras |
| `TIP_EFI` | Tipo de reporte | Acepta repetidas |
