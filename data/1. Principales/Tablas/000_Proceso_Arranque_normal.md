# Arquitectura UML: Proceso de Inicialización de Sesión Cuando no es primera vez(Arranque)

Este documento explica de forma clara y estructurada el proceso interno que ocurre en milisegundos cuando un usuario abre la aplicación DATTA ERP. Este flujo actúa como un **"Motor Ensamblador"** que blinda la seguridad, carga la personalización del sistema y construye la "memoria caché" (`JSON_SES`) para evitar consultar al servidor constantemente.

A diferencia del diagrama de clases de la base de datos, en este escenario modelamos la **Estructura Orientada a Objetos** del motor de arranque. En UML, los procesos de Velneo se abstraen como *Clases Controlador* y el JSON como una *Clase DTO (Data Transfer Object)*.

```mermaid
classDiagram
    %% Capa de Memoria (Estructura de Datos)
    class JSON_SES {
        <<Objeto DTO / Caché de Memoria>>
        +Entero globalVar_CUR_USR_ID
        +Cadena globalVar_CUR_USR_NOM
        +Cadena globalVar_CUR_USR_GRP
        +Booleano globalVar_CRR_APL
        +JSON registro_004_CFG_APP
        +JSON registro_004_TEM
        +Array lista_PRM
    }
    
    %% Capa de Controladores (Scripts/Procesos)
    class ON_INIT_CLIENT {
        <<Evento / Entry Point>>
        +LanzarSES_DAT()
        +EvaluarCierre(globalVar_CRR_APL)
        +PersistirEnVariableLocal()
    }
    
    class SES_DAT {
        <<Proceso de Servidor (Síncrono)>>
        +CargarConfigAPP()
        +CargarTemaUI()
        +LlamarAutoexec()
        +ConsolidarPermisosUsuario()
    }
    
    class AUTOEXEC {
        <<Proceso Autenticador>>
        +BuscarUsuarioSistema(sysUserName)
        +ExtraerNodosGrupo()
        +BloquearAccesoInvalido()
    }
    
    %% Capa de Base de Datos (Entidades referenciadas)
    class Tablas_DB {
        <<Tablas>>
        +004_CFG_APP
        +004_TEM
        +001_USU
        +001_USU_GRP
        +001_GRP_USU
        +001_PRM
    }

    %% Relaciones Estructurales UML
    ON_INIT_CLIENT "1" *-- "1" JSON_SES : Instancia y Persiste
    SES_DAT "1" o-- "1" JSON_SES : Mutación (Inyecta Config)
    AUTOEXEC "1" o-- "1" JSON_SES : Mutación (Inyecta Perfil)
    
    %% Relaciones de Dependencia (Calls)
    ON_INIT_CLIENT ..> SES_DAT : <<call>>
    SES_DAT ..> AUTOEXEC : <<call>>
    
    %% Dependencias de Lectura
    SES_DAT ..> Tablas_DB : <<read>>
    AUTOEXEC ..> Tablas_DB : <<read>>
```

## Explicación Arquitectónica (Semántica UML)

1. **Creación y Propiedad (`*--`):** 
   El proceso principal `ON_INIT_CLIENT` es el "Dueño" de la variable local `JSON_SES`. Él la crea al inicio y, al final del flujo, la deposita en la Variable Global `SES_DAT` de la memoria RAM del ERP para que cualquier pantalla pueda leer los permisos sin ir a la base de datos.
   
2. **Mutación de Datos (`o--`):** 
   Los procesos de servidor `SES_DAT` y `AUTOEXEC` solo reciben la variable JSON prestada. Su único trabajo es buscar información en la base de datos, meter esa información dentro de la variable (añadiéndole campos como `lista_PRM`, `globalVar_CUR_USR_ID` y `globalVar_CRR_APL`) y devolverla.

3. **Dependencias de Lectura (`..>`):** 
   Ninguno de los procesos modifica la base de datos física (`Tablas_DB`), solo ejecutan secuencias de *Read* navegando por los enlaces plurales (desde el usuario `001_USU` hasta los permisos granulares `001_PRM`) para poder recolectar los privilegios en una Cesta.

---

## Diagrama de Casos de Uso: Acceso al Sistema

Bajo los estándares de arquitectura UML estricta, un Caso de Uso debe representar una **meta valiosa para un actor externo**, no los pasos técnicos o cronológicos de un proceso interno. Por lo tanto, el proceso técnico de "Arranque y armado del JSON" es un medio para un fin mayor: **Acceder al Sistema**.

```mermaid
flowchart LR
    classDef actor fill:#eceff1,stroke:#455a64,stroke-width:2px
    classDef usecase fill:#e3f2fd,stroke:#1565c0,stroke-width:2px,shape:ellipse

    %% Actores Verdaderos (Externos al Sistema)
    Usuario(("👤 Usuario Operador")):::actor

    subgraph "DATTA ERP - Límite del Sistema"
        %% El caso de uso es la META del usuario
        Login(["Acceder al Sistema\n(Iniciar Aplicación)"]):::usecase
        
        %% Caso de uso incluido (Reutilizable/Mandatorio)
        Auth(["Validar Identidad"]):::usecase
        
        %% Caso de extensión (Flujo alterno de excepción)
        Bloquear(["Denegar Acceso"]):::usecase
    end

    %% Relaciones
    Usuario ---> Login
    
    %% Inclusión: Para acceder, el sistema obligatoriamente valida identidad
    Login -. "<<include>>" .-> Auth
    
    %% Extensión: Bloquear extiende a Validar Identidad SOLO si falla la condición
    Bloquear -. "<<extend>>\n[Condición: Credenciales inválidas o\nUsuario no registrado en OS]" .-> Auth
```

### Arquitectura de Negocio vs. Procesos Técnicos

1. **El Actor Verdadero:** El único actor real en este escenario es el `Usuario`. El sistema no se "autoconsume" en un caso de uso. Elementos como el "Caja de Base de Datos" pertenecen a la capa de componentes.
2. **Uso estricto de `<<include>>`:** Para cumplir su meta de `Acceder al Sistema`, el comportamiento del caso de uso obliga a ejecutar mandatoriamente el proceso de `Validar Identidad` (que por debajo ejecutará `AUTOEXEC` y consolidará el JSON de sesión).
3. **Flujo de Excepción (`<<extend>>`):** El caso de uso `Denegar Acceso` (cerrar la app) es un flujo excepcional. Solo entra en acción interrumpiendo a `Validar Identidad` cuando se cumple la condición de falla.

*(Nota: La lógica técnica interna de cómo el sistema construye la sesión JSON y ejecuta los scripts se detalla a continuación).*

---

## Diagrama de Secuencia: Flujo de Inicialización

Aquí abandonamos la visión de negocio y entramos a la **visión técnica estricta**. El Diagrama de Secuencia muestra las "Líneas de Vida" de los componentes a lo largo del tiempo, detallando **qué se ejecuta primero, quién llama a quién (síncrono) y cómo viaja la memoria**.

```mermaid
sequenceDiagram
    autonumber
    
    %% Definición de Participantes (Líneas de Vida)
    actor U as Usuario Operador
    participant C as ON_INIT_CLIENT<br/><<Cliente / Frontend>>
    participant S as SES_DAT<br/><<Proceso Servidor>>
    participant A as AUTOEXEC<br/><<Proceso Autenticador>>
    participant DB as Base de Datos<br/><<Sistema Físico>>

    %% Disparador
    U->>C: Abrir Aplicación DATTA ERP
    activate C
    
    %% Instanciación Local
    C->>C: Crear Variable (JSON_SES_Vacío)
    
    %% Llamada al servidor (3er plano)
    C->>S: LanzarProceso( JSON_SES )
    activate S
    
    %% Carga de Configuración
    S->>DB: Leer() [004_CFG_APP, 004_TEM]
    activate DB
    DB-->>S: retornar( Configuración y Tema )
    deactivate DB
    S->>S: Mutar JSON (Inyectar UI)
    
    %% Delegación a Autenticación
    S->>A: LanzarProceso( JSON_SES )
    activate A
    
    %% Verificación de Usuario
    A->>DB: BuscarUsuario( sysUserName ) en 001_USU
    activate DB
    DB-->>A: retornar( Ficha de Usuario o Nulo )
    deactivate DB
    
    %% Bloque Lógico Condicional (ALT UML Estricto)
    alt Usuario NO existe
        A->>A: Mutar JSON (globalVar_CRR_APL = 1)
    else Usuario SÍ existe
        A->>DB: Cargar Plurales() [001_USU_GRP]
        activate DB
        DB-->>A: retornar( Grupos Asignados )
        deactivate DB
        A->>A: Mutar JSON (Inyectar Perfil)
    end
    
    %% Retorno de Autenticación
    A-->>S: retornar( JSON_SES_Modificado )
    deactivate A
    
    %% Consolidación de Permisos
    S->>DB: Cargar Plurales() [Navegar hasta 001_PRM]
    activate DB
    DB-->>S: retornar( Reglas de Permisos )
    deactivate DB
    
    S->>S: Agregar a Cesta y Mutar JSON (lista_PRM)
    
    %% Retorno Final al Cliente
    S-->>C: retornar( JSON_SES_COMPLETO )
    deactivate S
    
    %% Persistencia en RAM Global
    C->>C: Modificar Variable Local( SES_DAT = JSON_SES_COMPLETO )
    
    %% Decisión Final de Apertura
    alt JSON_SES.globalVar_CRR_APL == 1
        C-->>U: Mostrar Error ("Usuario no autorizado") y Abortar
    else JSON_SES.globalVar_CRR_APL != 1
        C-->>U: Renderizar Interfaz Principal (Arranque Exitoso)
    end
    deactivate C
```

### Arquitectura Técnica en el Tiempo
1. **Líneas de Vida (`activate/deactivate`):** `ON_INIT_CLIENT` se mantiene activo durante todo el proceso. Es el "Hilo Principal". Los procesos de servidor (`SES_DAT` y `AUTOEXEC`) solo se activan temporalmente para hacer su trabajo y mueren al devolver la respuesta.
2. **El Bloque ALT (La Seguridad):** Aquí modelamos el momento en que el servidor asigna el error (`globalVar_CRR_APL = 1`) en la capa de Autenticación, y cómo el Cliente intercepta ese veredicto al final para denegarle la pantalla al usuario.

---

## Diagrama de Actividad: Algoritmo Lógico

Mientras que el Diagrama de Secuencia muestra la línea de tiempo, el **Diagrama de Actividad** representa estrictamente el **algoritmo matemático (flujo lógico)**. 

Este diagrama implementa el patrón de arquitectura **Fail-Fast (Falla Rápido)**, asegurando que el sistema sea defensivo yorte de forma temprana si ocurre un fallo crítico de seguridad.

```mermaid
flowchart TD
    %% Estilos UML estrictos
    classDef startEnd fill:#000,stroke:#000,color:#fff
    classDef action fill:#fff3e0,stroke:#ff9800,stroke-width:2px
    classDef decision fill:#e1bee7,stroke:#8e24aa,stroke-width:2px

    Start(("Inicio: Ejecución de ON_INIT_CLIENT")):::startEnd
    EndOk(("Fin: Interfaz Principal Renderizada")):::startEnd
    EndErr(("Fin: App Abortada")):::startEnd

    %% Acciones
    Init["Crear Variable JSON_SES en Memoria"]:::action
    CargarUI["Leer Configuración (004_CFG) y UI (004_TEM)"]:::action
    Autenticar["Buscar sysUserName en Tabla 001_USU"]:::action
    
    %% Rombo de Decisión Único
    DecUser{"¿Usuario Existe en 001_USU?"}:::decision
    
    %% RUTA DE ERROR (Salida Inmediata / Fail-Fast)
    ShowErr["Mostrar Alerta: 'Usuario no autorizado'"]:::action
    
    %% RUTA EXITOSA (Continuidad Segura)
    LoadProfile["Inyectar ID, Nombre y Grupos al JSON"]:::action
    LoadPerms["Consolidar Permisos de BD (001_PRM) en una Cesta"]:::action
    SavePerms["Convertir Cesta e inyectar al JSON (lista_PRM)"]:::action
    SaveGlobal["Guardar JSON_SES en Variable Global Permanente"]:::action

    %% Flujo
    Start --> Init
    Init --> CargarUI
    CargarUI --> Autenticar
    Autenticar --> DecUser
    
    %% Bifurcación Limpia
    DecUser -->|No| ShowErr
    ShowErr --> EndErr
    
    DecUser -->|Sí| LoadProfile
    LoadProfile --> LoadPerms
    LoadPerms --> SavePerms
    SavePerms --> SaveGlobal
    SaveGlobal --> EndOk
```

### Análisis del Algoritmo

1. **Optimización por Retorno Temprano (Fail-Fast):** El algoritmo mitiga el desperdicio de ciclos de CPU en el servidor deteniendo el flujo en el primer punto de falla. Si el usuario es inválido, el sistema aborta de inmediato, impidiendo que se ejecuten consultas relacionales complejas hacia las tablas de permisos.
2. **Eliminación de Evaluaciones Redundantes:** Al no unificar los caminos de usuarios válidos e inválidos, el flujo se mantiene puro. Esto elimina la necesidad de banderas de control secundarias o de un segundo rombo de validación antes del renderizado de la UI, reduciendo la complejidad ciclomática del software.
3. **Documentación como Arquitectura:** Documentar cómo está escrito un mal proceso es bitácora de código; documentar cómo el sistema resuelve el problema con la estructura lógica más eficiente es verdadera Arquitectura de Software.
