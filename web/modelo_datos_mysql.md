# 🗄️ Modelo de Datos: MySQL (SaaS Orchestrator)

Este documento describe la estructura de la base de datos MySQL, la cual actúa como el **Orquestador SaaS** para el ecosistema Datta-Erp. Su función principal es gestionar usuarios, roles y la vinculación con las instancias de Velneo Cloud.

## 📊 Diccionario de Tablas

### 1. Gestión de Accesos (`roles` & `usuarios`)
Tablas encargadas de la seguridad y el control de acceso al portal web.

| Tabla | Propósito | Campos Clave |
| :--- | :--- | :--- |
| `roles` | Definición de niveles de acceso (Admin, Usuario, etc.). | `id`, `nombre_rol`, `es_activo` |
| `usuarios` | Registro central de usuarios con soporte para OTP. | `id`, `usuario`, `correo`, `verificado`, `codigo_otp` |

### 2. Infraestructura Velneo (`velneo_carpetas` & `velneo_instancias`)
Tablas que modelan la infraestructura física/lógica en los servidores de Velneo.

| Tabla | Propósito | Campos Clave |
| :--- | :--- | :--- |
| `velneo_carpetas` | Agrupación lógica de instancias para balanceo. | `nombre`, `limite_maximo`, `estado` |
| `velneo_instancias` | Registro detallado de instancias DAT y APP. | `id_instancia`, `proyecto`, `tipo` (dat/app) |

### 3. Vinculación (`velneo`)
Esta tabla es el **corazón de la integración**, uniendo al usuario web con su entorno ERP en Velneo.

- `id_usuario`: Relación con la tabla `usuarios`.
- `id_instancia_dat` / `id_instancia_app`: Punteros a las bases de datos y aplicaciones Velneo asignadas.
- `url_api`: Punto de enlace para el consumo de servicios REST del ERP.

---

## 🛠️ Flujo de Aprovisionamiento (Planificado)

1. **Registro**: El usuario se registra en la web (`usuarios`).
2. **Validación**: Se envía y verifica un `codigo_otp` via `MailModule`.
3. **Asignación**: El sistema busca una `velneo_carpeta` con estado `disponible`.
4. **Instanciación**: Se vinculan las `velneo_instancias` correspondientes al usuario en la tabla `velneo`.
5. **Acceso**: El usuario accede al Frontend, el cual consulta la `url_api` para interactuar con su ERP personalizado.

---

## 🔴 Reglas de Integridad Críticas
- Un usuario solo puede tener una instancia DAT y una APP vinculada activamente.
- Las URLs de API (`url_api`) deben ser únicas para evitar colisiones de tráfico entre clientes.
- El borrado de un usuario debe manejar en cascada (o restringir) sus vínculos con Velneo para evitar instancias huérfanas.
