# Tabla: ID: velneo

## 📄 Información General
- Tipo de tabla: Maestro
- Reside en: Disco
- Longitud del registro: 1500
- Número de campos: 14
- Número de índices: 8

## 📝 Descripción [IA]
Es el "Cerebro de Conexión" de la arquitectura multi-tenant de Datta-Erp. Esta tabla consolida toda la información necesaria para que un usuario acceda a su entorno privado de trabajo. Vincula de forma indisoluble al usuario con su carpeta cloud, sus instancias de datos y aplicación, y sus credenciales de seguridad en Velneo (Grupos y Usuarios de vServer). Sin la información contenida aquí, el Inyector Dinámico no puede autorizar ni enrutar las peticiones API del Dashboard.

## 🛠️ Estructura de Campos

| Identificador | Nombre | Tipo | Longitud | Tipo de enlace | Comentarios y Relaciones |
| :--- | :--- | :--- | :--- | :--- | :--- |
| `id` | ID Config | Numérico | 10 | | Clave primaria. |
| `id_usuario` | Usuario | Numérico | 10 | Maestro: `usuarios` | El dueño de este entorno de trabajo. |
| `id_carpeta` | Carpeta | Numérico | 10 | Maestro: `velneo_carpetas` | Ubicación física del tenant en el cloud. |
| `id_instancia_dat` | Instancia DAT | Numérico | 10 | Maestro: `velneo_instancias` | Referencia a los datos del cliente. |
| `id_instancia_app` | Instancia APP | Numérico | 10 | Maestro: `velneo_instancias` | Referencia a la lógica de la aplicación. |
| `cd_group` | ID Grupo V | Numérico | 11 | | Código numérico del grupo de seguridad en Velneo. |
| `id_group` | Alias Grupo | Alfa | 100 | | Nombre del grupo asignado al tenant. |
| `id_group_check` | Grupo OK | Booleano | 1 | | Confirmación de que el grupo fue creado exitosamente (1 = Sí). |
| `cd_user` | ID Usuario V | Numérico | 11 | | Código numérico del usuario técnico en Velneo. |
| `id_user_velneo` | Alias User V | Alfa | 100 | | Identificador del usuario para autenticación REST. |
| `id_user_check` | User V OK | Booleano | 1 | | Confirmación de que el usuario técnico fue creado (1 = Sí). |
| `url_api` | Endpoint API | Alfa | 255 | | URL única y aislada para las peticiones de este cliente. |
| `fecha_creacion` | Aprovisionado | Tiempo | 4 | | Fecha del despliegue completo del entorno. |
| `fecha_actualizacion` | Último Cambio | Tiempo | 4 | | Registro de modificaciones en la configuración del tenant. |

## 🔍 Índices

| Identificador | Nombre | Tipo de índice | Campos |
| :--- | :--- | :--- | :--- |
| `PRIMARY` | Primary Key | Clave única | `id` |
| `uk_id_group` | Grupo Único | Clave única | `id_group` |
| `uk_id_user_velneo` | User V Único | Clave única | `id_user_velneo` |
| `uk_url_api` | URL Única | Clave única | `url_api` |
| `uk_instancia_dat` | DAT Única | Clave única | `id_instancia_dat` |
| `uk_instancia_app` | APP Única | Clave única | `id_instancia_app` |
| `idx_velneo_usuario` | Dueño | Acepta repetidas | `id_usuario` |
| `idx_velneo_carpeta` | Ubicación | Acepta repetidas | `id_carpeta` |

## 🔗 Enlaces Plurales
- *No reporta enlaces plurales salientes.*

## 📌 Notas [IA]
- Esta tabla es la más sensible del Hub. Cualquier error en los campos de `url_api` o IDs de instancia romperá la conexión del cliente con su ERP.
- Los campos `check` son fundamentales para el reintento automático en caso de fallos parciales durante el proceso de aprovisionamiento.
- Se utiliza `ON DELETE RESTRICT` para evitar dejar instancias en el cloud sin una configuración que las gestione.