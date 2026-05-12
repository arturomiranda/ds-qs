# Tabla: ID: 004_CFG_APP

## 📄 Información General
- Tipo de tabla: Maestro
- Reside en: Disco
- Longitud del registro: 85
- Número de campos: 6
- Número de índices: 2

## 📝 Descripción
Esta tabla almacena los parámetros de configuración global que rigen el comportamiento de la interfaz y la operación del ERP a nivel de aplicación. A diferencia de otras tablas de configuración más complejas, esta versión simplificada se enfoca en aspectos críticos como la identidad de la aplicación (`APP_NOM`), la preferencia visual activa (`TEM`) y controles de sesión y estado de la barra. Es el primer punto de consulta del sistema al iniciar la interfaz de usuario.

## 🛠️ Estructura de Campos

| Identificador | Nombre | Tipo | Longitud | Tipo de enlace | Comentarios y Relaciones |
| :--- | :--- | :--- | :--- | :--- | :--- |
| `ID` | Código | Numérico | 1 | | Identificador único (usualmente registro único). |
| `APP_NOM` | Nombre de la aplicación | Alfa 256 | 40 | | Nombre que se mostrará en las cabeceras de ventana. |
| `TEM` | Tema por defecto | Alfa 256 | 40 | Maestro: `004_TEM@datta_erp_dat` | Enlace al tema visual activo de la aplicación. |
| `LUC_ON` | Control de usuarios concurrentes activo | Booleano | 1 | | Activa o desactiva la validación de sesiones múltiples. |
| `OCU_BAR_EST` | Ocultar la barra de estado | Booleano | 1 | | Control visual para maximizar el área de trabajo. |
| `MOD_REC` | Modo de reconexión | Alfa 256 | 1 | Estática: `MOD_REC_W@datta_erp_dat` | Define el comportamiento del vClient ante desconexiones. |

## 🔍 Índices

| Identificador | Nombre | Tipo de índice |
| :--- | :--- | :--- |
| `ID` | Código | Acepta repetidas |
| `TEM` | Tema | Múltiples claves |

## 🔗 Enlaces Plurales
*No reporta enlaces plurales.*

## ⚡ Triggers
*No reporta triggers definidos.*

## 📌 Notas
Dado que esta tabla suele contener un único registro maestro, se recomienda acceder a ella mediante un proceso de "Cargar Ficha" al arranque de la aplicación para persistir estos valores en variables globales. El enlace con la tabla `004_TEM` asegura que cualquier cambio en la hoja de estilo CSS se aplique de forma reactiva en la siguiente sesión del usuario.
