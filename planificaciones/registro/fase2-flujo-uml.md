# FASE 2 — Flujo Principal UML
> Módulo: **Registro de Instancias Velneo + Usuario**
> Arquitectura: Híbrida con 3 Planos de Ejecución — Redis + BullMQ + SessionKeeper Cron

---

## Arquitectura: 3 Planos de Ejecución

```mermaid
flowchart TD
    subgraph Plano1["🔵 PLANO 1 — Request HTTP (Síncrono ~ms)"]
        A([POST /registration/initiate]) --> B[DTO Validation\nzod/joi schema]
        B --> C{¿Email existe\nen MySQL local?}
        C -- Sí + verificado --> ERR1[/"❌ 400: Cuenta activa"/]:::error
        C -- No --> D[GET sessionToken\ndesde Redis Cache]
        D --> E{¿Token\ncacheado?}
        E -- No → Fallback --> F[Crear sesión Velneo\nde emergencia]
        E -- Sí → ~ms --> G[checkUserExists\nVelneo Cloud API]
        F --> G
        G --> H{¿Existe en\nVelneo Cloud?}
        H -- Sí --> ERR2[/"❌ 400: Email en Cloud"/]:::error
        H -- No --> I[Generar OTP + username\n+ autoPassword]
        I --> J[Firmar registrationToken\nJWT TTL 15min]
        J --> K[📧 Enqueue Mail Job\n→ BullMQ:mail-queue]
        K --> L([✅ 202: registrationToken])
    end

    subgraph Plano2["🟠 PLANO 2 — Background Workers (Asíncrono)"]
        M([POST /registration/verify]) --> N[Verificar JWT\n+ OTP correcto]
        N --> O{¿Válido?}
        O -- No --> ERR3[/"❌ 400: Token/OTP inválido"/]:::error
        O -- Sí --> P[💾 withTransaction\ncrearUsuario + velneoRecord\nCheckpoint-0]
        P --> Q[Enqueue Provisioning Job\n→ BullMQ:provisioning-queue\ncon provisioningId]
        Q --> R([✅ 200: provisioningId\n+ statusUrl])
    end

    subgraph Plano3["🟢 PLANO 3 — Session Keeper Cron (Background perpetuo)"]
        S([⏱️ Cron: cada 25 min]) --> T[Crear nueva sesión\nVelneo Cloud]
        T --> U[Autenticar vServer]
        U --> V[SET sessionToken en Redis\nTTL: 29 min]
        V --> W([✅ Token vivo en cache])
    end

    D -.->|Lee| V
    K -.->|Encola| X[["📬 MailWorker\nenvía OTP"]]
    Q -.->|Encola| Y[["⚙️ ProvisioningWorker\nCheckpoints 1→4"]]

    classDef error fill:#fee2e2,stroke:#f87171,color:#7f1d1d
```

---

## Diagrama de Secuencia — Flujo Completo Multi-Actor

```mermaid
sequenceDiagram
    actor U as 👤 Usuario
    participant FE as Frontend
    participant API as HTTP Layer
    participant RS as RegistrationService
    participant Redis as 🗄️ Redis
    participant BQ as 📬 BullMQ
    participant DB as 🐬 MySQL
    participant PW as ⚙️ ProvisioningWorker
    participant SK as ⏱️ SessionKeeper Cron
    participant VC as ☁️ Velneo Cloud API
    participant MAIL as 📧 SMTP

    Note over SK, VC: Proceso perpetuo en background
    loop Cada 25 minutos
        SK->>VC: POST /session + PUT /vserver
        VC-->>SK: sessionToken fresco
        SK->>Redis: SET velneo:session TTL 29min
    end

    U->>FE: Completa formulario
    FE->>API: POST /registration/initiate
    API->>RS: initiate(dto)
    RS->>DB: findUserByEmail(email)
    DB-->>RS: null
    RS->>Redis: GET velneo:session
    Redis-->>RS: sessionToken (~1ms)
    RS->>VC: checkUserExists(token, email)
    VC-->>RS: false
    RS->>RS: Generar OTP + username + autoPassword
    RS->>RS: jwt.sign(pendingData, TTL 15min)
    RS->>BQ: mail-queue.add({ email, OTP })
    BQ-->>RS: jobId
    RS-->>API: { registrationToken }
    API-->>FE: 202 { registrationToken }
    BQ--)MAIL: [MailWorker] sendOTP(email)
    MAIL-->>U: 📧 Correo con OTP

    U->>FE: Ingresa código OTP
    FE->>API: POST /registration/verify
    API->>RS: verify(email, code, token)
    RS->>RS: jwt.verify + comparar OTP
    RS->>DB: withTransaction → INSERT usuarios + velneo
    DB-->>RS: { userId, velneoRecordId }
    RS->>BQ: provisioning-queue.add({ userId, velneoRecordId, userData })
    BQ-->>RS: provisioningId
    RS-->>API: { provisioningId, statusUrl }
    API-->>FE: 200 { provisioningId, statusUrl }

    Note over PW, VC: Worker procesa en background
    BQ--)PW: [ProvisioningWorker] job dequeued
    PW->>Redis: GET velneo:session
    Redis-->>PW: sessionToken
    PW->>VC: PUT /instance (data) → Checkpoint 1
    PW->>DB: updateVelneoStatus(check_dat=1)
    PW->>VC: PUT /instance (app) → Checkpoint 2
    PW->>DB: updateVelneoStatus(check_app=1)
    PW->>VC: PUT /group → Checkpoint 3
    PW->>DB: updateVelneoStatus(check_group=1)
    PW->>VC: PUT /user → Checkpoint 4
    PW->>DB: updateVelneoStatus(check_user=1)
    PW->>BQ: mail-queue.add({ email, credentials })
    BQ--)MAIL: [MailWorker] sendCredentials(email)
    MAIL-->>U: 📧 Credenciales de acceso
```

---

## Módulo Transversal: `core/infrastructure/` — Workers Aislados

```mermaid
flowchart LR
    subgraph Core["🔵 core/infrastructure (Módulo Transversal)"]
        direction TB
        QM["QueueManager\nconexión BullMQ + Redis"]
        SM["SessionKeeper\nCron: sesión Velneo viva"]
        MW["MailWorker\nProcesa mail-queue"]
        PW["ProvisioningWorker\nProcesa provisioning-queue"]
        RC["RedisClient\nSingleton compartido"]
    end

    subgraph Modules["🟠 Módulos de Dominio"]
        RS["RegistrationService\nenqueue jobs → no procesa"]
        PS["ProvisioningService\nlógica de checkpoints"]
    end

    RS -->|add job| QM
    QM --> MW
    QM --> PW
    PW --> PS
    SM --> RC
    PW --> RC
    RS --> RC

    style Core fill:#fff7ed,stroke:#f97316
    style Modules fill:#dbeafe,stroke:#3b82f6
```

---

## Decisiones de Diseño Clave

| Decisión | Elección | Razón |
|---|---|---|
| Cola de trabajos | **BullMQ** (sobre Redis) | Mismo Redis para session cache + queue; menos infra |
| Retry automático | BullMQ `attempts: 3, backoff: exponential` | Recupera checkpoints fallidos sin intervención manual |
| Session Keeper | **Cron cada 25 min** (TTL Redis: 29 min) | Margen de seguridad sin dejar el token expirar |
| Fallback de sesión | Crear sesión de emergencia si Redis falla | El `/initiate` nunca cae por ausencia del cron |
| Workers | **Procesos aislados** en `core/workers/` | No bloquean el event loop del servidor HTTP |

> 🔴 **Punto crítico:** Si Redis cae, el cron pierde el token cacheado y el fallback entra en juego creando sesiones directas a Velneo Cloud. Redis debe monitorearse como servicio crítico.
