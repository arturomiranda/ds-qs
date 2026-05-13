# 📋 Módulo: Registro y Aprovisionamiento de Instancias Velneo

> **Responsabilidad:** Orquesta el ciclo de vida completo de un nuevo suscriptor SaaS — desde la validación de identidad (stateless con OTP) hasta el aprovisionamiento automatizado de instancias de datos, aplicación, grupos de seguridad y cuenta de usuario en Velneo Cloud.
>
> **Fuera de scope:** Gestión de sesiones activas, renovación de tokens, administración de carpetas en el servidor Velneo (debe ser manual).

---

## 📁 Archivos Clave

| Capa | Archivo | Responsabilidad |
|---|---|---|
| Ruta | `Backend/src/modules/Auth/routes/Auth.routes.js` | Define los endpoints HTTP públicos del flujo |
| Controlador | `Backend/src/modules/Auth/controllers/Auth.controller.js` | Valida entradas y mapea respuestas HTTP |
| Servicio | `Backend/src/modules/Auth/services/Auth.service.js` | Orquestador principal de la lógica de negocio |
| Proveedor | `Backend/src/modules/Auth/providers/Auth.provider.js` | Acceso y persistencia en MySQL (usuarios, velneo, carpetas) |
| Infraestructura | `Backend/src/modules/Velneo/services/Velneo.service.js` | Integración con el API REST de Velneo Cloud |

---

## 1. Flujo Principal

```mermaid
flowchart TD
    A([Cliente envía POST /auth/register]) --> B{¿Usuario existe\ny verificado?}
    B -- Sí, con infraestructura --> E1[/"❌ Error: Cuenta activa"/]:::error
    B -- No o incompleto --> C[Validar email en Velneo Cloud]
    C --> D{¿Existe en vServer?}
    D -- Sí --> E2[/"❌ Error: Email vinculado a Cloud"/]:::error
    D -- No --> F[Generar username, password y OTP]
    F --> G[Firmar registrationToken JWT\n⏳ TTL 15 min]
    G --> H[📧 Enviar OTP por correo]
    H --> I([Respuesta: 201 + registrationToken])

    I --> J([Cliente envía POST /auth/verify-email])
    J --> K{¿JWT válido\ny OTP correcto?}
    K -- No --> E3[/"❌ Error: Token expirado o código incorrecto"/]:::error
    K -- Sí --> L[💾 Persistir usuario en MySQL\n withTransaction]
    L --> M[Asignar carpeta disponible]
    M --> N[Crear registro velneo inicial\nCheckpoint 0]
    N --> O[🔑 Autenticar sesión Velneo Cloud]
    O --> P[Checkpoint 1:\ncrearInstanciaDatos]
    P --> Q[Checkpoint 2:\ncrearInstanciaApp]
    Q --> R[Checkpoint 3:\nconfigurarGrupos]
    R --> S[Checkpoint 4:\ncrearUsuarioCloud]
    S --> T[📧 Enviar credenciales al usuario]
    T --> U([Respuesta: 200 - Aprovisionamiento completo])

    P -. rollback .-> E4[/"⚠️ Fallo parcial: estado guardado en velneo"/]:::error
    Q -. rollback .-> E4
    R -. rollback .-> E4
    S -. rollback .-> E4

    classDef error fill:#fee2e2,stroke:#f87171,color:#7f1d1d
```

---

## 2. Arquitectura de Capas

```mermaid
flowchart TD
    subgraph Frontend["🖥️ Frontend (Next.js)"]
        direction LR
        F1["RegisterForm\napp/page.tsx"]
        F2["VerifyOtpForm\napp/page.tsx"]
    end

    subgraph Backend["⚙️ Backend (Node/Express)"]
        direction TB
        R["Auth.routes.js\nPOST /register\nPOST /verify-email"]
        C["Auth.controller.js"]
        S["Auth.service.js"]
        P["Auth.provider.js"]
        VS["Velneo.service.js"]
        MS["Mail.service.js"]
    end

    subgraph External["☁️ Externos"]
        direction TB
        VC["Velneo Cloud API\nvelneoCloudClient"]
        DB[("MySQL\nusuarios · velneo\nvelneo_carpetas")]
        MAIL["Nodemailer / SMTP"]
    end

    F1 -->|POST /register| R
    F2 -->|POST /verify-email| R
    R --> C --> S
    S --> P --> DB
    S --> VS --> VC
    S --> MS --> MAIL

    style Frontend fill:#dbeafe,stroke:#3b82f6,color:#1e3a8a
    style Backend fill:#f0fdf4,stroke:#22c55e,color:#14532d
    style External fill:#fff7ed,stroke:#f97316,color:#7c2d12
```

---

## 3. Esquema de Base de Datos

```mermaid
erDiagram
    USUARIOS {
        int id PK
        varchar nombres
        varchar apellido_paterno
        varchar apellido_materno
        varchar correo UK
        varchar usuario UK
        varchar password
        varchar num_telefono
        varchar empresa
        varchar rfc
        tinyint verificado
        tinyint debe_cambiar_password
        datetime fecha_registro
    }

    VELNEO {
        int id PK
        int id_usuario FK
        int id_carpeta FK
        int cd_ins_dat
        int cd_ins_app
        int cd_group
        int cd_user
        varchar id_instancia_app
        varchar id_instancia_dat
        varchar id_group
        varchar id_user_velneo
        varchar url_api
        tinyint id_instancia_app_check
        tinyint id_instancia_dat_check
        tinyint id_group_check
        tinyint id_user_check
    }

    VELNEO_CARPETAS {
        int id PK
        varchar nombre UK
        int contador_instancias
        enum estado
    }

    USUARIOS ||--o| VELNEO : "tiene"
    VELNEO_CARPETAS ||--o{ VELNEO : "agrupa"
```

---

## 4. Contrato de API

### `POST /backend/auth/register`

Inicia el flujo de registro. **No persiste al usuario.** Solo valida y envía el OTP.

| Campo | Tipo | Requerido | Descripción |
|---|---|---|---|
| `firstName` | `string` | ✅ | Nombre(s) del usuario |
| `lastNameP` | `string` | ✅ | Apellido paterno |
| `lastNameM` | `string` | ❌ | Apellido materno |
| `email` | `string` | ✅ | Correo institucional (único) |
| `phone` | `string` | ❌ | Teléfono de contacto |
| `company` | `string` | ❌ | Empresa |
| `rfc` | `string` | ❌ | RFC del cliente |

**Respuesta exitosa `201`:**
```json
{
  "success": true,
  "message": "Código de verificación enviado al correo",
  "data": {
    "email": "usuario@empresa.com",
    "registrationToken": "<JWT>",
    "verificationRequired": true
  }
}
```

**Errores controlados `400`:**
```json
{ "success": false, "message": "Este correo ya tiene una cuenta activa y configurada." }
{ "success": false, "message": "El correo ya está vinculado a una cuenta en Velneo Cloud." }
```

---

### `POST /backend/auth/verify-email`

Valida el OTP, persiste al usuario y aprovisiona toda la infraestructura Cloud.

| Campo | Tipo | Requerido | Descripción |
|---|---|---|---|
| `email` | `string` | ✅ | Correo a verificar |
| `code` | `string` | ✅ | OTP de 6 dígitos recibido por correo |
| `registrationToken` | `string` | ✅ | JWT devuelto en el paso anterior |

**Respuesta exitosa `200`:**
```json
{
  "success": true,
  "message": "Correo verificado e infraestructura aprovisionada",
  "data": {
    "email": "usuario@empresa.com",
    "velneo": {
      "success": true,
      "folder": "clientes01",
      "username": "juan.perez123",
      "appInstanceId": 42,
      "dataInstanceId": "INS_DAT_001"
    }
  }
}
```

**Errores controlados `400`:**
```json
{ "success": false, "message": "El tiempo para verificar ha expirado. Regístrate de nuevo." }
{ "success": false, "message": "El código de verificación es incorrecto" }
```

---

## 5. Diagrama de Secuencia — Aprovisionamiento Cloud

```mermaid
sequenceDiagram
    actor U as Usuario
    participant FE as Frontend
    participant C as Auth.controller
    participant S as Auth.service
    participant P as Auth.provider
    participant VS as Velneo.service
    participant DB as MySQL
    participant VC as Velneo Cloud API
    participant MAIL as SMTP

    U->>FE: Completa formulario de registro
    FE->>C: POST /auth/register {userData}
    C->>S: register(userData)
    S->>P: findUserByEmail(email)
    P->>DB: SELECT usuarios WHERE correo = ?
    DB-->>P: null / usuario
    P-->>S: existingUser

    S->>VS: createSession()
    VS->>VC: POST /session
    VC-->>VS: sessionToken
    VS->>VC: GET /vserver + PUT /vserver (auth)
    S->>VS: checkUserExists(sessionToken, email)
    VS->>VC: GET /users
    VC-->>VS: false
    VS->>VC: DELETE /session
    S->>MAIL: sendVerificationCode(email, OTP)
    MAIL-->>U: 📧 Correo con OTP
    S-->>C: { registrationToken, email }
    C-->>FE: 201 { registrationToken }

    U->>FE: Ingresa código OTP
    FE->>C: POST /auth/verify-email {email, code, registrationToken}
    C->>S: verifyEmail(email, code, token)
    S->>S: jwt.verify(token) → decoded
    S->>P: withTransaction → createUser + createInitialVelneoRecord
    P->>DB: INSERT usuarios + INSERT velneo
    DB-->>P: userId, velneoRecordId

    S->>VS: createSession() + authenticateVServer()
    VS->>VC: POST /session + PUT /vserver

    S->>VS: createDataInstance(token, email, folder)
    VS->>VC: PUT /instance (type=data)
    VC-->>VS: dataInstanceId
    S->>P: updateVelneoStatus(check_dat=1)

    S->>VS: createAppInstance(token, email, folder, dataId)
    VS->>VC: PUT /instance (type=app, inheritance)
    VC-->>VS: appCodigo, idInstancia
    S->>P: updateVelneoStatus(check_app=1)

    S->>VS: setupGroups(token, email, idInstancia)
    VS->>VC: PUT /group (Administradores + tenant)
    VC-->>VS: groupCodigo
    S->>P: updateVelneoStatus(check_group=1)

    S->>VS: createUserAccount(token, userData)
    VS->>VC: PUT /user
    VC-->>VS: vUsername, userCodigo
    S->>P: updateVelneoStatus(check_user=1)

    VS->>VC: DELETE /session
    S->>MAIL: sendCredentials(email, username, password)
    MAIL-->>U: 📧 Credenciales de acceso
    S-->>C: { velneo: { folder, username, appInstanceId } }
    C-->>FE: 200 Aprovisionamiento completo
```

---

## 6. Mecanismo de Checkpoints (Idempotencia)

El flujo de aprovisionamiento utiliza un sistema de **Checkpoints** que permite reanudar un proceso fallido sin duplicar recursos en Velneo Cloud.

| Checkpoint | Campo en `velneo` | Verifica |
|---|---|---|
| `0` | Registro inicial creado | `id_usuario` presente |
| `1` | Instancia de Datos | `id_instancia_dat_check = 1` |
| `2` | Instancia de Aplicación | `id_instancia_app_check = 1` |
| `3` | Grupo de Seguridad | `id_group_check = 1` |
| `4` | Usuario Cloud | `id_user_check = 1` |

> ⚠️ **Anti-patrón detectado:** Si el proceso falla en el Checkpoint 2 o posterior, la sesión Velneo Cloud sigue activa hasta el bloque `finally`. En un volumen alto de registros concurrentes, esto puede agotar el número de sesiones simultáneas permitidas por la API. Se recomienda evaluar un mecanismo de cola (BullMQ) para serializar el aprovisionamiento.

---

## 7. Lógica de Asignación de Carpetas

Las instancias de Velneo se organizan en **carpetas físicas** del servidor. El sistema aplica la siguiente lógica:

1. Consulta `velneo_carpetas` en MySQL buscando una con `estado = 'disponible'`.
2. Si no existe en DB, consulta la API de Velneo Cloud filtrando carpetas con prefijo `VELNEO_BASE_FOLDER_NAME`.
3. Una carpeta se marca como `'llena'` al alcanzar **10 instancias** (`MAX_INSTANCES_PER_FOLDER`).
4. Las carpetas **nunca se crean automáticamente** — deben ser provisionadas manualmente en el servidor.

> ⚠️ **Punto de fallo manual:** Si todas las carpetas están llenas y no existe una nueva creada en el servidor, el proceso lanza una excepción no recuperable.

---

## 8. Seguridad

| Control | Implementación |
|---|---|
| Verificación de identidad | OTP de 6 dígitos con TTL de 15 min (JWT firmado con `JWT_SECRET`) |
| Contraseña en tránsito | Generada automáticamente, hasheada con `bcrypt` (10 salt rounds) antes de persistir en MySQL |
| Contraseña en Cloud | Se transmite en texto plano al API de Velneo Cloud — **tráfico debe ser HTTPS** |
| Doble validación | El email se verifica tanto en MySQL local como en el vServer de Velneo Cloud antes de proceder |
| Credenciales de Cloud | Se leen exclusivamente de variables de entorno (`.env`) — nunca hardcodeadas |

---

## 9. Variables de Entorno Requeridas

| Variable | Uso |
|---|---|
| `JWT_SECRET` | Firma y verificación de tokens de registro y sesión |
| `VELNEO_CLOUD_EMAIL` | Cuenta administradora para crear sesiones en el API de Velneo |
| `VELNEO_VSERVER_USER` | Usuario del vServer para el handshake de autenticación |
| `VELNEO_VSERVER_PASSWORD` | Contraseña del vServer |
| `VELNEO_BASE_FOLDER_NAME` | Prefijo de las carpetas de clientes (ej. `cloud`) |
| `VELNEO_SOLUTION` | Identificador de la solución Velneo desplegada |
| `VELNEO_DATA_PROJECT` | Proyecto de datos a instanciar |
| `VELNEO_APP_PROJECT` | Proyecto de aplicación a instanciar |
| `VELNEO_APP_VCD` | Identificador VCD para la herencia de la instancia de app |

---

## 📊 Resumen del Módulo

| Métrica | Valor |
|---|---|
| Endpoints documentados | 2 (`/register`, `/verify-email`) |
| Entidades en el ER | 3 (`usuarios`, `velneo`, `velneo_carpetas`) |
| Pasos en el flujo principal | 12 |
| Servicios externos | 2 (Velneo Cloud API, SMTP) |
| Checkpoints de idempotencia | 4 |
| TTL del token de registro | 15 minutos |
