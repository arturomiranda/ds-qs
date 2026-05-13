# Tabla: ID: velneo

## 📄 Información General
- Tipo de tabla: Maestro
- Reside en: Disco
- Longitud del registro: 850
- Número de campos: 13
- Número de índices: 8

## 📝 Descripción [IA]
Esta es la tabla de orquestación técnica del SaaS. Su función es mapear cada usuario verificado con su correspondiente infraestructura en Velneo Cloud. Actúa como un registro de estado (State Machine) para el aprovisionamiento, almacenando los IDs de grupos, usuarios y carpetas asignados en el vServer. Es el componente crítico para la comunicación entre el backend Node.js y la API de Velneo.

## 🛠️ Estructura de Campos

| Identificador | Nombre | Tipo | Longitud | Tipo de enlace | Comentarios y Relaciones |
| :--- | :--- | :--- | :--- | :--- | :--- |
| `id` | ID Velneo | Numérico | 10 | | Clave primaria. |
| `id_usuario` | Usuario | Numérico | 10 | Maestro: `usuarios` | El dueño de la infraestructura. |
| `id_carpeta` | Carpeta | Numérico | 10 | Maestro: `velneo_carpetas` | Carpeta física asignada en Velneo. |
| `id_instancia_dat` | Instancia DAT | Numérico | 10 | Maestro: `velneo_instancias` | Referencia a la caja de datos del tenant. |
| `id_instancia_app` | Instancia APP | Numérico | 10 | Maestro: `velneo_instancias` | Referencia a la caja de aplicación del tenant. |
| `cd_group` | Cód. Grupo | Numérico | 11 | | Código interno devuelto por Velneo Cloud. |
| `id_group` | ID Grupo | Alfa | 100 | | Identificador textual del grupo (SaaS Tenant). |
| `id_group_check` | Grupo Creado | Booleano | 1 | | Flag de confirmación de creación del grupo. |
| `cd_user` | Cód. Usu. Vel | Numérico | 11 | | Código interno del usuario en Velneo. |
| `id_user_velneo` | Login Velneo | Alfa | 100 | | Username del suscriptor en el vServer. |
| `id_user_check` | Usu. Creado | Booleano | 1 | | Flag de confirmación de creación del usuario. |
| `url_api` | URL API | Alfa | 255 | | Endpoint personalizado para el acceso del cliente. |
| `fecha_creado` | Aprovisionado el | Tiempo | 4 | | Timestamp del éxito total del proceso. |

## 🔍 Índices

| Identificador | Nombre | Tipo de índice |
| :--- | :--- | :--- |
| `PRIMARY` | Primary Key | Clave única (id) |
| `uk_id_group` | Grupo Único | Clave única (id_group) |
| `uk_id_user_velneo` | Login Único | Clave única (id_user_velneo) |
| `uk_url_api` | URL Única | Clave única (url_api) |
| `uk_instancia_dat` | DAT Única | Clave única (id_instancia_dat) |
| `uk_instancia_app` | APP Única | Clave única (id_instancia_app) |
| `idx_usuario` | Ref Usuario | Acepta repetidas (id_usuario) |
| `idx_carpeta` | Ref Carpeta | Acepta repetidas (id_carpeta) |

## 🔗 Enlaces Plurales
- No tiene enlaces plurales directos en este esquema.

## 📌 Notas [IA]
- El uso de múltiples `UK` (Unique Keys) asegura que no haya colisiones de infraestructura entre tenants.
- Los campos `*_check` son vitales para sistemas de reintento automático (Workers/BullMQ) en caso de fallos parciales durante el aprovisionamiento.
- Un registro completo en esta tabla es garantía de que el suscriptor puede entrar a usar el ERP.
