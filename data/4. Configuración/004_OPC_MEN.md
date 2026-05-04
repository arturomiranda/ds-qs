# Tabla: 004: Opciones de menú

Esta tabla maestra de clave arbolada define la estructura jerárquica del menú principal de la aplicación. Permite configurar dinámicamente el acceso a formularios, procesos y reportes.

## 📄 Información General
- **ID de Tabla:** `004_OPC_MEN`
- **Tipo:** Maestro con clave arbolada
- **Reside en:** Disco
- **Longitud del registro:** 545
- **Número de campos:** 10
- **Número de índices:** 6
- **Descripción:** Definición jerárquica de las opciones de menú del sistema.

## 🛠️ Estructura de Campos

| Identificador | Nombre | Tipo | Longitud | Tipo de enlace |
| :--- | :--- | :--- | :--- | :--- |
| `ID` | Código | Alfa 40 | 12 | |
| `NAME` | Nombre | Alfa 256 | 40 | |
| `TOO_TIP` | Texto en tooltip | Alfa 256 | 100 | |
| `OBJ_ID_REF` | idRef del objeto | Alfa 256 | 100 | |
| `ID_PAD` | Id Padre | Alfa 256 | 40 | |
| `STA_TIP` | Texto en la barra de estado | Alfa 256 | 100 | |
| `ICO_ID_REF` | idRef del icono | Alfa 256 | 100 | |
| `MEN_OBJ` | Tipo de objeto | Alfa 256 | 1 | Estática: PRS_MEN_TIP_W@datta_erp_dat |
| `OBJ_TIP` | Tipo de objeto | Numérico | 1 | |
| `OBJ_ID` | Objeto | Alfa 256 | 50 | |

## 🔍 Índices

| Identificador | Nombre | Tipo de indice |
| :--- | :--- | :--- |
| `ID` | Código | Clave única |
| `NAME` | Alfabético | Acepta repetidas |
| `WORDS` | Palabras | Palabras |
| `PARTS` | Trozos de palabras | Trozos de palabras |
| `OBJ_TIP_ID_REF` | Tipo + idRef del objeto | Acepta repetidas |
| `OBJ_ID_REF` | idRef del objeto | Acepta repetidas |

## 📝 Notas
- La estructura arbolada (`ID_PAD`) permite crear submenús con niveles ilimitados.
- El campo `OBJ_ID_REF` vincula la opción de menú con el objeto técnico (Formulario, Rejilla, Proceso) que se debe ejecutar.
- El campo `MEN_OBJ` utiliza una tabla estática para clasificar la opción (Carpeta, Formulario, Acción, etc.).
