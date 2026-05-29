# Tabla: ID: 004_OPC_MEN

## 📄 Información General
- Tipo de tabla: Maestro con clave arbolada
- Reside en: Disco
- Longitud del registro: 545
- Número de campos: 10
- Número de índices: 6

## 📝 Descripción
Tabla maestra para la generación dinámica de la interfaz de usuario. Almacena la estructura jerárquica del menú principal. Gracias a su naturaleza de "clave arbolada", permite definir menús, submenús y opciones de ejecución con niveles ilimitados de profundidad. Se vincula con el sistema de permisos para mostrar u ocultar opciones según el perfil del usuario, y con los objetos técnicos (Formularios/Procesos) que deben dispararse al hacer clic.

## 🛠️ Estructura de Campos

| Identificador | Nombre | Tipo | Longitud | Tipo de enlace | Comentarios y Relaciones |
| :--- | :--- | :--- | :--- | :--- | :--- |
| `ID` | Código | Alfa 40 | 12 | | Clave arbolada que define la posición en la jerarquía. |
| `NAME` | Nombre | Alfa 256 | 40 | | Texto que se mostrará en el menú de la aplicación. |
| `TOO_TIP` | Texto en tooltip | Alfa 256 | 100 | | Ayuda visual que aparece al pasar el cursor sobre la opción. |
| `OBJ_ID_REF` | idRef del objeto | Alfa 256 | 100 | | Referencia técnica al objeto a ejecutar (ej. "alias/FORM_ENT"). |
| `ID_PAD` | Id Padre | Alfa 256 | 40 | | Enlace a la opción de nivel superior en el árbol. |
| `STA_TIP` | Texto en la barra de estado | Alfa 256 | 100 | | Mensaje informativo que se muestra en la parte inferior del ERP. |
| `ICO_ID_REF` | idRef del icono | Alfa 256 | 100 | | Referencia a la imagen o icono SVG para la opción. |
| `MEN_OBJ` | Tipo de objeto | Alfa 256 | 1 | Estática: `PRS_MEN_TIP_W@datta_erp_dat` | Clasificación del objeto (Carpeta, Formulario, Acción, Proceso). |
| `OBJ_TIP` | Tipo de objeto | Numérico | 1 | | Clasificador numérico secundario para el motor de UI. |
| `OBJ_ID` | Objeto | Alfa 256 | 50 | | Nombre corto o alias del objeto técnico. |

## 🔍 Índices

| Identificador | Nombre | Tipo de índice |
| :--- | :--- | :--- |
| `ID` | Código | Clave única |
| `NAME` | Alfabético | Acepta repetidas |
| `WORDS` | Palabras | Palabras |
| `PARTS` | Trozos de palabras | Trozos de palabras |
| `OBJ_TIP_ID_REF` | Tipo + idRef del objeto | Acepta repetidas |
| `OBJ_ID_REF` | idRef del objeto | Acepta repetidas |

## 🔗 Enlaces Plurales
*No reporta enlaces plurales directos en el esquema físico.*

## ⚡ Triggers
*No definidos.*

## 📌 Notas
La lógica de visualización del menú debe filtrar esta tabla consultando simultáneamente la tabla de permisos (`001_PRM`). Es una tabla crítica para la personalización de la experiencia de usuario (UX). Se recomienda que el campo `ICO_ID_REF` utilice iconos normalizados para mantener la estética premium del sistema.
