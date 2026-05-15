# Tabla: ID: 004_APP_CFG_W
# Nombre: 004: Configuraciones de la aplicación

## 📄 Información General
- Tipo de tabla: Maestro
- Reside en: Disco
- Longitud del registro: 131
- Número de campos: 10
- Número de índices: 2

## 📝 Descripción
Esta tabla centraliza las configuraciones globales de comportamiento y apariencia de la aplicación. Actúa como el panel de control técnico para definir variables de entorno, tiempos de animación de la interfaz y parámetros de persistencia de conexión. Es una tabla de tipo "Singleton" (diseñada para un registro único) que es consultada por el motor de arranque del ERP para configurar la experiencia del usuario final.

## 🛠️ Estructura de Campos

| Identificador | Nombre | Tipo | Longitud | Tipo de enlace | Comentarios y Relaciones |
| :--- | :--- | :--- | :--- | :--- | :--- |
| `ID` | Código | Numérico | 1 | | Identificador único. Su longitud 1 confirma el uso de un solo registro de configuración. |
| `APP_NOM` | Nombre de la aplicación | Alfa 256 | 40 | | Título global que se muestra en las ventanas y reportes del sistema. |
| `TEM_COM` | Tema compacto | Alfa 256 | 40 | | Identificador del tema visual optimizado para pantallas pequeñas. |
| `TEM` | Tema por defecto | Alfa 256 | 40 | | Identificador del tema visual principal (enlazado a `004_TEM`). |
| `ANI_DCK_MS` | Milisegundos animación docks | Numérico | 2 | | Controla la velocidad de despliegue de los paneles laterales. |
| `LUC_ON` | Control de usuarios concurrentes activo | Booleano | 1 | | Activa la verificación de límites de licencias en tiempo real. |
| `OCU_BAR_EST` | Ocultar la barra de estado | Booleano | 1 | | Ajuste de UI para maximizar el área de trabajo útil. |
| `MOD_REC` | Modo de reconexión | Alfa 256 | 1 | Estática: `MOD_REC_W@datta_erp_dat` | Define la política de reconexión ante fallos de red. |
| `SEG_REC` | Segundos entre reintentos de reconexión | Numérico | 2 | | Intervalo de espera para intentar restablecer la sesión. |
| `ANI_FRM_MS` | Milisegundos animación formularios | Numérico | 2 | | Suavidad de las transiciones visuales al navegar entre opciones. |

## 🔍 Índices

| Identificador | Nombre | Tipo de índice |
| :--- | :--- | :--- |
| `ID` | Código | Clave única |
| `TEM` | Tema | Múltiples claves |

## 🔗 Enlaces Plurales
*No reporta enlaces plurales.*

## ⚡ Triggers
- **Alta:** Posterior a un alta de ficha
- **Modificación:** Posterior a una modificación de ficha
- **Baja:** Posterior a una baja de ficha

## 📌 Notas
Cualquier cambio en esta tabla tiene un impacto inmediato en la usabilidad del sistema. Se recomienda restringir el acceso a esta tabla únicamente a usuarios con perfil de "Administrador de Sistemas". Los triggers posteriores sugieren que el sistema realiza una limpieza de caché o reinicio de variables globales tras actualizar estos parámetros.
