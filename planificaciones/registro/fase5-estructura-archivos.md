# FASE 5 — Estructura de Archivos
> Módulo: **Registro de Instancias Velneo + Usuario**
> Estándar: **Feature-Based Architecture** — separación estricta entre dominio, infraestructura transversal y capa HTTP

---

## Backend — Árbol de Directorios

```
Landing/Backend/src/
│
├── 📁 core/                               ← Infraestructura transversal (sin lógica de negocio)
│   ├── 📁 database/
│   │   ├── baseSql.provider.js            ← Base ORM ligero (existente)
│   │   ├── transactionSql.js              ← withTransaction helper (existente)
│   │   └── pool.js                        ← Singleton conexión MySQL
│   │
│   ├── 📁 redis/
│   │   ├── redis.client.js                ← Singleton RedisClient (ioredis)
│   │   └── redis.encrypt.js               ← AES-256 para valores sensibles en Redis
│   │
│   ├── 📁 queue/
│   │   ├── queue.manager.js               ← Instancias de Queue (provisioning, mail)
│   │   └── queue.constants.js             ← Nombres de colas como constantes
│   │
│   ├── 📁 workers/
│   │   ├── provisioning.worker.js         ← Consume provisioning-queue
│   │   ├── mail.worker.js                 ← Consume mail-queue
│   │   └── worker.bootstrap.js            ← Arranca todos los workers
│   │
│   ├── 📁 cron/
│   │   └── session.keeper.js              ← Cron BullMQ cada 25min → renueva token Velneo
│   │
│   ├── 📁 errors/
│   │   ├── AppError.js                    ← Clase base de errores controlados
│   │   └── error.handler.js               ← Middleware global de errores Express
│   │
│   └── 📁 http/
│       └── baseHttp.provider.js           ← Cliente HTTP base (existente, renombrado)
│
├── 📁 modules/
│   │
│   ├── 📁 Registration/                   ← DOMINIO 1: Identidad y OTP
│   │   ├── 📁 controllers/
│   │   │   └── Registration.controller.js
│   │   ├── 📁 services/
│   │   │   └── Registration.service.js    ← initiate() + verify()
│   │   ├── 📁 providers/
│   │   │   └── Registration.provider.js   ← SQL: usuarios, carpetas
│   │   ├── 📁 dtos/
│   │   │   ├── InitiateRegistration.dto.js ← Schema Zod
│   │   │   └── VerifyEmail.dto.js          ← Schema Zod
│   │   ├── 📁 routes/
│   │   │   └── Registration.routes.js
│   │   └── index.js                        ← Barrel export del módulo
│   │
│   ├── 📁 Provisioning/                   ← DOMINIO 2: Aprovisionamiento Cloud
│   │   ├── 📁 controllers/
│   │   │   └── Provisioning.controller.js  ← status() + retry()
│   │   ├── 📁 services/
│   │   │   └── Provisioning.service.js     ← lógica de checkpoints
│   │   ├── 📁 providers/
│   │   │   └── Provisioning.provider.js    ← SQL: velneo record CRUD
│   │   ├── 📁 routes/
│   │   │   └── Provisioning.routes.js
│   │   └── index.js
│   │
│   ├── 📁 Velneo/                         ← DOMINIO 3: API Client Velneo Cloud (SRP)
│   │   ├── 📁 services/
│   │   │   ├── VelneoSession.service.js    ← createSession, authenticate, close
│   │   │   ├── VelneoInstance.service.js   ← createDataInstance, createAppInstance
│   │   │   ├── VelneoGroup.service.js      ← setupGroups
│   │   │   └── VelneoUser.service.js       ← checkUserExists, createUserAccount
│   │   ├── 📁 providers/
│   │   │   └── VelneoHttp.provider.js      ← BaseHttpProvider especializado
│   │   └── index.js                        ← Barrel: exporta todos los sub-servicios
│   │
│   ├── 📁 Auth/                           ← DOMINIO 4: Sesiones activas (Login/Logout)
│   │   ├── 📁 controllers/
│   │   │   └── Auth.controller.js
│   │   ├── 📁 services/
│   │   │   └── Auth.service.js             ← login(), logout(), changePassword()
│   │   ├── 📁 providers/
│   │   │   └── Auth.provider.js
│   │   ├── 📁 routes/
│   │   │   └── Auth.routes.js
│   │   └── index.js
│   │
│   ├── 📁 Mail/                           ← Sin cambios (ya aislado)
│   ├── 📁 Menu/                           ← Sin cambios
│   ├── 📁 Catalogos/                      ← Sin cambios
│   └── 📁 Notifications/                  ← Sin cambios
│
├── 📁 middlewares/
│   ├── auth.middleware.js                  ← authenticate (existente)
│   ├── isAdmin.middleware.js               ← NUEVO: verifica rol admin
│   ├── validate.middleware.js              ← NUEVO: ejecuta schema Zod/Joi
│   └── rateLimiter.middleware.js           ← NUEVO: límites por endpoint
│
├── 📁 config/
│   ├── httpClients.js                      ← velneoCloudClient (existente)
│   ├── bullmq.config.js                   ← NUEVO: configuración BullMQ
│   └── redis.config.js                    ← NUEVO: opciones ioredis
│
├── app.js                                  ← Express app (existente)
├── server.js                               ← Entry point API HTTP
├── router.js                               ← Root router (actualizar rutas)
└── worker.js                               ← NUEVO: Entry point exclusivo para workers
```

---

## Separación de Entry Points — `server.js` vs `worker.js`

| Proceso | Entry Point | Responsabilidad | Reinicio independiente |
|---|---|---|---|
| API HTTP | `server.js` | Atiende requests Express, puerto 3000 | ✅ Sí |
| Workers | `worker.js` | ProvisioningWorker + MailWorker + SessionKeeper | ✅ Sí |

> **Beneficio clave:** Si un worker falla o necesita reiniciarse, el servidor HTTP sigue operando sin interrupción.

---

## Desglose del módulo `Velneo/` — Responsabilidad Única

| Archivo | Responsabilidad | Métodos principales |
|---|---|---|
| `VelneoSession.service.js` | Ciclo de vida de sesiones | `createSession()`, `authenticateVServer()`, `closeSession()` |
| `VelneoInstance.service.js` | Instancias de datos y app | `createDataInstance()`, `createAppInstance()`, `getOrCreateActiveFolder()`, `ensureFolderExists()` |
| `VelneoGroup.service.js` | Grupos de seguridad | `setupGroups()` |
| `VelneoUser.service.js` | Usuarios en vServer | `checkUserExists()`, `createUserAccount()` |

> Resuelve el 🔴 **SRP violation** del estado actual: `Velneo.service.js` tenía 384 líneas con 4 responsabilidades mezcladas.

---

## Frontend — Árbol de Directorios

```
Landing/Frontend/
│
├── 📁 app/
│   ├── 📁 (public)/                       ← Rutas sin autenticación
│   │   ├── 📁 registro/
│   │   │   └── page.tsx                    ← Paso 1: formulario de datos
│   │   └── 📁 verificar/
│   │       └── page.tsx                    ← Paso 2: ingreso de OTP
│   ├── 📁 (auth)/                          ← Rutas protegidas
│   │   └── 📁 dashboard/
│   │       └── page.tsx
│   └── layout.tsx
│
├── 📁 modules/
│   └── 📁 Registration/
│       ├── 📁 components/
│       │   ├── RegistrationForm.tsx        ← Paso 1: datos personales
│       │   ├── OtpVerifyForm.tsx           ← Paso 2: ingreso OTP
│       │   └── ProvisioningStatus.tsx      ← Paso 3: polling de estado
│       ├── 📁 hooks/
│       │   ├── useRegistration.ts          ← Llama a initiate()
│       │   ├── useOtpVerify.ts             ← Llama a verify()
│       │   └── useProvisioningStatus.ts    ← Polling /status/:id cada 3s
│       ├── 📁 services/
│       │   └── registration.api.ts         ← Fetch calls al backend
│       └── 📁 types/
│           └── registration.types.ts       ← Interfaces TypeScript
│
├── 📁 components/                          ← Componentes globales reutilizables
├── 📁 providers/                           ← Context providers
├── 📁 helpers/                             ← Sin cambios
└── 📁 hooks/                               ← Hooks globales
```

---

## Comparativa: Estado Actual vs Propuesto

| Aspecto | Estado Actual | Estado Propuesto |
|---|---|---|
| Módulos de dominio | 1 (`Auth`) mezcla registro + aprovisionamiento + login | 4 separados: `Registration`, `Provisioning`, `Velneo`, `Auth` |
| Velneo service | 1 clase, 384 líneas, 4 responsabilidades | 4 servicios especializados (~80 líneas c/u) |
| Validación de entrada | Ninguna (controller directo a service) | DTOs con Zod por endpoint + middleware `validate` |
| Workers | Inline en el request HTTP (bloquea event loop) | Procesos aislados en `core/workers/` vía `worker.js` |
| Redis | No utilizado | Session cache (SessionKeeper) + BullMQ queues |
| Entry points | `server.js` (todo en uno) | `server.js` API + `worker.js` workers — independientes |
| Rate limiting | No existe | Middleware por endpoint con ventanas diferenciadas |
| Manejo de errores | Try/catch por controlador | `AppError` centralizado + middleware global `error.handler.js` |

---

## Router Principal — Actualización Requerida

```
/api/v1/registration  → Registration.routes.js   (público)
/api/v1/provisioning  → Provisioning.routes.js   (autenticado + isAdmin para retry)
/api/v1/auth          → Auth.routes.js            (público login/logout)
/api/v1/menu          → Menu.routes.js            (autenticado)
/api/v1/catalogo      → Catalogos.routes.js       (autenticado)
/api/v1/notifications → Notification.routes.js    (autenticado)
```
