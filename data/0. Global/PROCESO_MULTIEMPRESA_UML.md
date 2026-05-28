# 🏗️ Arquitectura del Sistema Multiempresa — DATTA ERP
> **Suite UML completa** | Generado por: `architect-uml-sat-velneo-sync` | Fuente: Ingeniería inversa de `DATTA_ERP_app` y `DATTA_ERP_dat`

---

## 📊 Análisis de Consistencia (Pre-Diagramación)

Entidades detectadas en el código fuente y su mapeo al modelo de datos:

| Componente de Código | Tabla / Variable | Rol en el Sistema |
|:---|:---|:---|
| `ON_INIT_CLIENT.vb` | — | Orquestador de arranque en el Cliente |
| `SES_DAT.vb` | `$SES_DAT@datta_erp_dat` | Builder del objeto JSON de sesión |
| `AUTOEXEC.vb` | `001_USU@datta_erp_dat` | Autenticador y lector de grupos |
| `AUTOEXEC_PRE_INI.vb` | `$CRR_APL`, `$CUR_USR_ID`, `$CUR_USR_NOM` | Hidratador de variables globales |
| `AUTOEXEC_POS_INI.vb` | `APL_TEM` (función) | Aplicador de tema CSS |
| `APL_TEM.vb` | `004_TEM@datta_erp_dat` | Inyector de hoja de estilo al Marco |
| `PRM_TBL.vb` | `001_PRM@datta_erp_dat` | Motor de validación RBAC en tiempo real |
| `PRE_INI.vb` (Marco) | — | Dispatcher del PRE_INI al 1° plano (local) |
| `POS_INI.vb` (Marco) | — | Dispatcher del POS_INI + oculta STATUS_BAR |

---

## 1️⃣ Diagrama de Casos de Uso

```mermaid
%%{init: {"theme": "base", "themeVariables": {"primaryColor": "#e1f5fe", "edgeLabelBackground":"#ffffff"}}}%%
graph LR
  subgraph "Sistema Operativo vClient"
    UC_LOGIN["fa:fa-user Iniciar Sesión"]
    UC_SEL_EMP["fa:fa-building Seleccionar Empresa Activa"]
    UC_NAVEGAR["fa:fa-compass Navegar Módulos ERP"]
    UC_OPERAR["fa:fa-edit Operar (Alta / Edición / Baja)"]
  end

  subgraph "Sistema Velneo — Servidor"
    UC_AUTENTICAR["fa:fa-key Autenticar Usuario\n(001_USU)"]
    UC_CARGAR_SES["fa:fa-database Construir JSON de Sesión\n(SES_DAT)"]
    UC_EVAL_PRM["fa:fa-shield Evaluar Permiso\n(PRM_TBL)"]
    UC_APLICAR_TEM["fa:fa-paint-brush Aplicar Tema CSS\n(APL_TEM)"]
  end

  USUARIO["👤 Usuario Final"] --> UC_LOGIN
  UC_LOGIN --> UC_AUTENTICAR
  UC_AUTENTICAR --> UC_CARGAR_SES
  UC_CARGAR_SES --> UC_SEL_EMP
  UC_SEL_EMP --> UC_NAVEGAR
  UC_NAVEGAR --> UC_OPERAR
  UC_OPERAR --> UC_EVAL_PRM
  UC_CARGAR_SES --> UC_APLICAR_TEM

  ADMIN["👤 Administrador"] --> UC_SEL_EMP
  ADMIN --> UC_EVAL_PRM
```

---

## 2️⃣ Diagrama de Clases — Anatomía del Modelo de Datos

```mermaid
classDiagram
  direction LR

  class `001_ENT` {
    +ID : Numérico PK
    +NAME : Alfa(256) - Razón Social
    +RFC : Alfa(13) - Unique
    +REG_FIS_SAT : Alfa(3)
    +CP_FIS : Alfa(5)
    +ES_EMPR : Booleano
    +TIP_NIV : Alfa(1)
    +ID_PAD : Numérico FK_self
    +UUID_EX : Alfa(40) - Unique
  }

  class `001_EMP` {
    +ID : Numérico PK
    +NAME : Alfa(35)
    +ENT : Numérico FK
    +ES_EMP : Booleano
    +EMP_PAD : Alfa(40)
    +CONF_SIS : ObjetoTexto - JSON
    +ACT : Booleano
  }

  class `001_USU` {
    +ID : Numérico PK
    +NAME : Alfa(60) - Unique - LoginKey
    +PSW_ACC : Alfa(256)
    +ENT_USU : Numérico FK
    +EXT : Booleano
    +OFF : Booleano
  }

  class `001_GRP_USU` {
    +ID : Numérico PK
    +NAME : Alfa(35)
    +DESC : Alfa(40)
    +OFF : Booleano
  }

  class `001_USU_EMP` {
    +ID : Numérico PK
    +USU : Numérico FK
    +EMP : Numérico FK
    +CRD_USR : Numérico FK
    +CRD_TIM : Tiempo
  }

  class `001_PRM` {
    +ID : Numérico PK
    +GRP : Numérico FK
    +TIP_OBJ : Alfa(1)
    +ID_TBL : Numérico FK
    +VER : Booleano
    +AGR : Booleano
    +EDT : Booleano
    +ELM : Booleano
    +CAN : Booleano
  }

  class `004_APP_CFG_W` {
    +ID : Numérico PK
    +APP_NOM : Alfa(40)
    +TEM : Alfa(40) FK
    +OCU_BAR_EST : Booleano
    +LUC_ON : Booleano
  }

  class `004_TEM` {
    +ID : Alfa(40) PK
    +NAME : Alfa(35)
    +CSS : ObjetoTexto
  }

  class `$SES_DAT` {
    <<Variable Global>>
    +globalVar_CUR_USR_ID : Numérico
    +globalVar_CUR_USR_NOM : Alfa
    +globalVar_CUR_USR_GRP : Alfa
    +globalVar_CRR_APL : Booleano
    +globalVar_CRR_APL_MSG : Alfa
    +globalVar_LOG_SES : Alfa
    +registro_004_CFG_APP : JSON
    +registro_004_TEM : JSON
    +lista_PRM : JSON Array
  }

  `001_ENT` "1" --> "0..*" `001_USU` : ENT_USU (FK)
  `001_EMP` "1" --> "0..*" `001_USU_EMP` : EMP

  `001_GRP_USU` "1" --> "0..*" `001_PRM` : GRP

  `004_APP_CFG_W` --> `004_TEM` : TEM (FK)

  `$SES_DAT` ..> `001_USU` : hidrata desde
  `$SES_DAT` ..> `001_PRM` : acumula permisos
  `$SES_DAT` ..> `004_APP_CFG_W` : serializa config
  `$SES_DAT` ..> `004_TEM` : serializa tema
```

---

## 3️⃣ Diagrama de Secuencia — Flujo Cronológico del Arranque

```mermaid
sequenceDiagram
  autonumber
  participant vClient as "fa:fa-desktop vClient (UI)"
  participant OnInit as "ON_INIT_CLIENT.vb"
  participant SesDat as "SES_DAT.vb (Servidor)"
  participant AutoExec as "AUTOEXEC.vb (Servidor)"
  participant PreIni as "AUTOEXEC_PRE_INI.vb"
  participant PosIni as "AUTOEXEC_POS_INI.vb"
  participant AplTem as "APL_TEM.vb"
  participant DB as "fa:fa-database datta_erp_dat"

  vClient->>OnInit: Evento ON_INIT_CLIENT disparado
  OnInit->>OnInit: Set JSON_SES.globalVar_LOG_SES = $LOG_SES
  OnInit->>SesDat: Crear pro_SES_DAT (3° plano Servidor, síncrono)
  OnInit->>SesDat: Set variable JSON_SES -> pro_SES_DAT
  SesDat->>DB: Cargar lista 004_CFG_APP (pos 1)
  DB-->>SesDat: Ficha 004_APP_CFG_W
  SesDat->>SesDat: Generar JSON_REG desde ficha
  SesDat->>SesDat: JSON_SES += "registro_004_CFG_APP"
  SesDat->>DB: Leer maestro TEM desde 004_APP_CFG_W
  DB-->>SesDat: Ficha 004_TEM (CSS)
  SesDat->>SesDat: JSON_SES += "registro_004_TEM"
  SesDat->>AutoExec: Crear pro_AUTOEXEC (3° plano Servidor, síncrono)
  AutoExec->>DB: Cargar lista 001_USU WHERE NAME = sysUserName
  alt Usuario NO encontrado
    DB-->>AutoExec: Lista vacía
    AutoExec->>AutoExec: JSON_SES.globalVar_CRR_APL = 1
    AutoExec->>AutoExec: JSON_SES.globalVar_CRR_APL_MSG = "No autorizado"
    AutoExec-->>SesDat: Retorno proceso = NO
  else Usuario encontrado
    DB-->>AutoExec: Ficha 001_USU
    AutoExec->>AutoExec: JSON_SES += "globalVar_CUR_USR_ID", "globalVar_CUR_USR_NOM"
    AutoExec->>DB: Cargar plurales 001_USU_GRP_USU
    DB-->>AutoExec: Lista de grupos del usuario
    AutoExec->>AutoExec: Iterar grupos → acumular USR_GRP_LIST
    AutoExec->>AutoExec: JSON_SES += "globalVar_CUR_USR_GRP"
    AutoExec-->>SesDat: JSON_SES enriquecido
  end
  SesDat->>SesDat: Get JSON_SES de pro_AUTOEXEC
  SesDat->>DB: Cesta local 001_PRM
  SesDat->>DB: Cargar lista 001_USU WHERE ID = ID_USU
  DB-->>SesDat: Ficha 001_USU
  SesDat->>DB: Cargar plurales 001_USU_GRP_USU
  DB-->>SesDat: Lista grupos
  loop Por cada Grupo
    SesDat->>DB: Leer maestro GRP (001_GRP_USU)
    SesDat->>DB: Cargar plurales 001_PRM_GRP
    DB-->>SesDat: Permisos del grupo
    SesDat->>SesDat: Cesta += lista de permisos
  end
  SesDat->>SesDat: Procesar Cesta → JSON Array lista_PRM
  SesDat->>SesDat: JSON_SES += "lista_PRM"
  SesDat-->>OnInit: JSON_SES completo devuelto
  OnInit->>DB: Modificar variable global SES_DAT = JSON_SES
  alt CRR_APL = 1 (usuario no autorizado)
    OnInit->>vClient: Retorno proceso = NO → App cierra
  else Autorizado
    OnInit-->>vClient: Continúa flujo PRE_INI
  end

  Note over vClient: Marco AUTOEXEC — Evento PRE_INI
  vClient->>PreIni: Crear proceso_pre_ini (1° plano local, síncrono)
  PreIni->>PreIni: Set JSON_SES = $SES_DAT
  PreIni->>DB: Modificar $CRR_APL = JSON_SES.globalVar_CRR_APL
  PreIni->>DB: Modificar $CUR_USR_ID = JSON_SES.globalVar_CUR_USR_ID
  PreIni->>DB: Modificar $CUR_USR_NOM = JSON_SES.globalVar_CUR_USR_NOM
  alt CRR_APL = 1
    PreIni->>vClient: Mensaje error → Finalizar proceso → Retorno = NO
  else OK
    PreIni-->>vClient: Variables globales hidratadas ✓
  end

  Note over vClient: Marco AUTOEXEC — Evento POS_INI
  vClient->>PosIni: Disparar AUTOEXEC_POS_INI
  PosIni->>PosIni: Set JSON_SES = $SES_DAT
  PosIni->>PosIni: Set JSON_APP_CFG_W = JSON_SES.registro_004_CFG_APP
  alt OCU_BAR_EST = 1
    PosIni->>vClient: Interfaz.Ocultar(STATUS_BAR)
  end
  PosIni->>AplTem: Llamar fun:APL_TEM (función)
  AplTem->>AplTem: Set JSON_TEM = $SES_DAT.registro_004_TEM
  AplTem->>AplTem: Set CSS = JSON_TEM.CSS
  AplTem->>AplTem: replaceString(CSS, "$sysCacheClientPath", sysCacheClientPath)
  AplTem->>vClient: Interfaz.EstablecerHojaEstiloCSS(Marco AUTOEXEC, CSS)
  vClient-->>vClient: 🎨 UI pintada con el tema activo
```

---

## 4️⃣ Diagrama de Actividad — Lógica Algorítmica del Arranque

```mermaid
flowchart TD
  START(["▶ App Velneo Inicia"]) --> A

  subgraph "Capa Cliente — ON_INIT_CLIENT"
    A["Prepara JSON_SES vacío\n+ inyecta globalVar_LOG_SES"] --> B
    B["Crea pro_SES_DAT\n(3° plano Servidor Síncrono)"] --> C
    C{"CRR_APL = 1?"}
    C -->|Sí| CERRAR["❌ Retorno = NO\nApp se cierra"]
    C -->|No| D["Persiste $SES_DAT\nen Variable Global"]
  end

  subgraph "Capa Servidor — SES_DAT.vb"
    B --> B1["Carga 004_CFG_APP → JSON_REG\nJSON_SES += registro_004_CFG_APP"]
    B1 --> B2["Lee maestro TEM\nJSON_SES += registro_004_TEM"]
    B2 --> B3["Crea pro_AUTOEXEC\n(3° plano Servidor Síncrono)"]
    B3 --> AUTH
  end

  subgraph "Capa Servidor — AUTOEXEC.vb (Autenticación)"
    AUTH{"001_USU.NAME\n= sysUserName?"}
    AUTH -->|No| AUTH_FAIL["JSON_SES.CRR_APL = 1\nJSON_SES.CRR_APL_MSG = 'No autorizado'\nRetorno = NO"]
    AUTH -->|Sí| AUTH_OK["JSON_SES += CUR_USR_ID, CUR_USR_NOM\nItera 001_USU_GRP_USU\nAcumula USR_GRP_LIST\nJSON_SES += CUR_USR_GRP"]
  end

  subgraph "Capa Servidor — SES_DAT.vb (Permisos)"
    AUTH_OK --> PRM1["Crea Cesta local 001_PRM"]
    PRM1 --> PRM2["Carga 001_USU → Navega\n001_USU_GRP_USU"]
    PRM2 --> PRM3{"¿Más grupos?"}
    PRM3 -->|Sí| PRM4["Sube a 001_GRP_USU\nBaja a 001_PRM_GRP\nAgregar a Cesta"]
    PRM4 --> PRM3
    PRM3 -->|No| PRM5["Procesar Cesta\nJSON_SES += lista_PRM (Array)"]
  end

  subgraph "Capa Cliente — Marco AUTOEXEC: PRE_INI"
    D --> E["Lee $SES_DAT → JSON_SES local"]
    E --> F["Hidrata variables globales:\n$CRR_APL, $CUR_USR_ID, $CUR_USR_NOM"]
    F --> G{"CRR_APL = 1?"}
    G -->|Sí| CERRAR
    G -->|No| H["✅ Variables globales listas"]
  end

  subgraph "Capa Cliente — Marco AUTOEXEC: POS_INI + APL_TEM"
    H --> I["Lee $SES_DAT → JSON_APP_CFG_W"]
    I --> J{"OCU_BAR_EST = 1?"}
    J -->|Sí| K["Ocultar STATUS_BAR"]
    J -->|No| L
    K --> L["Llamar fun:APL_TEM()"]
    L --> M["Lee $SES_DAT.registro_004_TEM\nExtrae campo CSS"]
    M --> N["replaceString CSS:\n$sysCacheClientPath → ruta real"]
    N --> O["Interfaz.EstablecerHojaEstiloCSS\n(Marco AUTOEXEC, CSS)"]
    O --> FIN(["🏁 ERP Listo para operar"])
  end

  AUTH_FAIL --> CERRAR
  PRM5 --> D
```

---

## 5️⃣ Diagrama Multiempresa — Flujo de Acceso por Empresa

```mermaid
flowchart LR
  subgraph "Modelo de Datos — Multitenancy"
    USU["fa:fa-user 001_USU\nUsuario"]
    USU_EMP["fa:fa-link 001_USU_EMP\nBridge Table"]
    EMP["fa:fa-building 001_EMP\nEmpresa / Sucursal"]
    ENT["fa:fa-id-card 001_ENT\nEntidad Legal RFC"]
    EMP_PAD["fa:fa-sitemap Jerarquía\nEMP_PAD → Empresa Matriz"]

    USU -->|"1..N"| USU_EMP
    USU_EMP -->|"N..1"| EMP
    EMP -->|"ENT (FK)"| ENT
    EMP -->|"ES_EMP=false"| EMP_PAD
    EMP_PAD -->|"es hija de"| EMP
  end

  subgraph "Sesión Activa — Variables Globales"
    GVAR["$SES_DAT\n─────────────\nglobalVar_CUR_USR_ID\nglobalVar_CUR_USR_NOM\nglobalVar_CUR_USR_GRP\nlista_PRM (JSON Array)"]
  end

  subgraph "Motor de Permisos — PRM_TBL.vb"
    PRM_TBL["fa:fa-shield PRM_TBL\n─────────────────────\nEntrada: NOM_TBL, ACCION\nLógica: CAT_TAB→001_USU\n→Grupos→001_PRM\nSalida: 0 (Denegado) / 1 (Permitido)"]
  end

  USU_EMP -->|"Determina acceso\na empresa"| GVAR
  GVAR -->|"Filtra datos operativos"| PRM_TBL

  style GVAR fill:#e8f5e9,stroke:#1b5e20
  style PRM_TBL fill:#f3e5f5,stroke:#4a148c
  style USU_EMP fill:#fff3e0,stroke:#e65100
  style ENT fill:#e1f5fe,stroke:#01579b
```

---

## 🧠 Notas de Arquitectura [IA]

### ✅ Fortalezas del Diseño

1. **Patrón JSON-as-Session-Bus:** El uso de `$SES_DAT` como un único objeto JSON serializado que transporta toda la información de sesión (usuario, config, permisos, tema) es elegante. Evita múltiples lecturas de base de datos en navegación y centraliza el estado.

2. **Separación de Planos (Cliente vs. Servidor):** `ON_INIT_CLIENT` corre en plano local y `SES_DAT`/`AUTOEXEC` en 3° plano servidor. Esto garantiza que la autenticación nunca se resuelva en el cliente, protegiendo la lógica de negocio.

3. **Cesta de Permisos Acumulativos:** La técnica de iterar todos los grupos del usuario y acumular permisos en una Cesta antes de serializar a JSON es un patrón RBAC robusto que soporta usuarios con múltiples roles sin conflictos.

4. **Soft Architecture para CSS:** Almacenar el CSS en `004_TEM.CSS` y aplicarlo en arranque mediante `APL_TEM` permite cambios de tema en caliente sin redeploy de la aplicación.

### ⚠️ Riesgos y Recomendaciones

| Riesgo | Impacto | Recomendación |
|:---|:---|:---|
| **`$SES_DAT` sin TTL explícito** | Si el JSON crece (muchos permisos), el rendimiento de `jsonGetValue` puede degradarse | Considerar indexación de permisos por clave compuesta `GRP_TIP_OBJ` dentro del JSON |
| **`001_USU_EMP` sin campo `EMP_ACTIVA`** | El usuario siempre debe seleccionar empresa al inicio. No hay "empresa por defecto" | Agregar campo `IS_DEFAULT` en `001_USU_EMP` para pre-seleccionar la empresa principal |
| **`PRM_TBL.vb` carga `001__USU_MTO`** | El proceso referencia una tabla `_MTO` que difiere de `001_USU`. Posible inconsistencia | Verificar si `001__USU_MTO` es un alias o tabla separada; alinear nomenclatura |
| **Permisos serialized en JSON vs. evaluados en tiempo real** | Lista `lista_PRM` se carga una sola vez en sesión. Cambios de permisos requieren re-login | Documentar este comportamiento; evaluar una función de recarga de sesión sin logout completo |

### 🔗 Alineación con `process-analyst`

| Variable en `process-analyst` | Mapeo en código fuente |
|:---|:---|
| `$G_EMP_ACTUAL` | Pendiente de implementación (recomendada en `001_EMP.md`) |
| `$G_SUC_ACTUAL` | Pendiente de implementación |
| `$SES_DAT` | Implementada en `ON_INIT_CLIENT.vb` → `$SES_DAT@datta_erp_dat` |
| `$CUR_USR_ID` | Implementada en `AUTOEXEC_PRE_INI.vb` |
| `$CUR_USR_NOM` | Implementada en `AUTOEXEC_PRE_INI.vb` |

> **⚡ Acción Pendiente:** Las variables `$G_EMP_ACTUAL` y `$G_SUC_ACTUAL` están documentadas en `001_EMP.md` como recomendación arquitectónica pero **no están implementadas** en ningún `.vb` analizado. Son el siguiente paso crítico para completar el flujo multiempresa operativo.
