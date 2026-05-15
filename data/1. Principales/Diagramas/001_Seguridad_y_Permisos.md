# Arquitectura UML: Módulo de Seguridad y Permisos (RBAC)

A continuación se presenta el Diagrama de Clases modelado con la sintaxis de UML para representar el ecosistema de seguridad basado en roles (Role-Based Access Control) del ERP. Este diagrama ilustra cómo los Usuarios (`001_USU`) se asocian a Grupos (`001_GRP_USU`), y cómo estos grupos administran Permisos granulares (`001_PRM`) sobre los Menús (`004_OPC_MEN`) y las Tablas de la base de datos (`004_CAT_TAB`).

```mermaid
classDiagram
    %% Relaciones de Asociación (Muchos a Muchos vía tabla intermedia)
    001_USU "1" -- "0..*" 001_USU_GRP : Asignado a
    001_GRP_USU "1" -- "0..*" 001_USU_GRP : Contiene
    
    %% Relaciones de Composición (Un grupo es dueño de sus permisos)
    001_GRP_USU "1" *-- "0..*" 001_PRM : Define privilegios
    
    %% Relaciones de Agregación (Los permisos apuntan a objetos del sistema)
    001_PRM "0..*" o-- "0..1" 004_OPC_MEN : Acceso a Menú
    001_PRM "0..*" o-- "0..1" 004_CAT_TAB : Acceso a Tabla
    
    %% Relación Jerárquica Reflexiva (Árbol de menús)
    004_OPC_MEN "1" o-- "0..*" 004_OPC_MEN : Sub-menús (ID_PAD)

    class 001_USU {
        <<Usuarios>>
        +Numérico ID
        +Alfa_60 NAME
        +Alfa_256 PSW_ACC
        +Numérico ENT
        +Booleano EXT
        +Booleano OFF
    }
    
    class 001_GRP_USU {
        <<Grupos de Usuarios>>
        +Numérico ID
        +Alfa_128 NAME
        +Alfa_256 DESC
    }
    
    class 001_USU_GRP {
        <<Usuarios por Grupo>>
        +Numérico USU
        +Numérico GRP
    }
    
    class 001_PRM {
        <<Permisos>>
        +Numérico ID
        +Numérico GRP
        +Alfa_1 TIP_OBJ
        +Alfa_12 ID_MENU
        +Numérico ID_TBL
        +Alfa_40 OPC_LIST
        +Booleano VER
        +Booleano AGR
        +Booleano EDT
        +Booleano ELM
        +Booleano CAN
    }
    
    class 004_OPC_MEN {
        <<Opciones de Menú>>
        +Alfa_12 ID
        +Alfa_256 NAME
        +Alfa_40 ID_PAD
        +Alfa_100 OBJ_ID_REF
        +Alfa_1 MEN_OBJ
    }
    
    class 004_CAT_TAB {
        <<Catálogo de Tablas>>
        +Numérico ID
        +Alfa_256 NAME
        +Alfa_256 IDE
    }
```

## Explicación Arquitectónica (Semántica UML)

1. **Gestión de Identidad y Roles (`001_USU` y `001_GRP_USU`)**
   * El sistema no asigna permisos directamente a los usuarios, sino que utiliza un modelo **RBAC (Role-Based Access Control)**. 
   * Existe una relación de muchos-a-muchos resuelta mediante la tabla asociativa `001_USU_GRP`. Un Usuario puede pertenecer a varios Grupos (ej. "Ventas" y "Gerencia"), y un Grupo lógicamente contiene múltiples Usuarios.

2. **Administración de Permisos Granulares (`001_PRM`)**
   * **Composición (`*--`):** La tabla de Permisos (`001_PRM`) tiene una relación de composición fuerte con el Grupo (`001_GRP_USU`). Si un grupo se elimina, todos los permisos definidos para ese grupo desaparecen.
   * La tabla de permisos utiliza booleanos de control de datos granulares (`VER`, `AGR`, `EDT`, `ELM`, `CAN`) que configuran exactamente qué acciones (CRUD) tiene permitido realizar el grupo.

3. **Restricción y Visibilidad de la Interfaz (`004_OPC_MEN` y `004_CAT_TAB`)**
   * **Agregación (`o--`):** Cada regla de permiso apunta a un objeto del sistema que desea proteger. Estos objetos existen independientemente de que tengan o no un permiso asignado, por eso la relación es de agregación.
   * El atributo `TIP_OBJ` dentro de `001_PRM` funciona como un enrutador polimórfico:
     * Si el permiso es para un menú, utiliza la llave foránea `ID_MENU` vinculándose a `004_OPC_MEN`. Esto dictamina qué elementos de la navegación lateral o superior puede ver el usuario.
     * Si el permiso es restrictivo a nivel de base de datos o listas, utiliza la llave foránea `ID_TBL` vinculándose al diccionario de datos en `004_CAT_TAB`. Esto permite evitar que un rol modifique registros físicos independientemente de a qué pantalla haya logrado entrar.

4. **Patrón de Árbol Jerárquico en Menús (`004_OPC_MEN`)**
   * **Agregación Reflexiva (`o--` hacia sí misma):** La tabla de Opciones de Menú contiene un campo `ID_PAD` (Id Padre) que apunta a un registro dentro de la misma tabla. Esto permite la construcción dinámica de sub-menús de nivel infinito (ej. Contabilidad > Reportes > Mensuales).

## Diagrama de Casos de Uso: Operación y Configuración

Para complementar la estructura técnica, a continuación se presenta un **Diagrama de Casos de Uso** que ilustra la funcionalidad del sistema desde la perspectiva del negocio y las interacciones entre los diferentes actores.

```mermaid
flowchart LR
    %% Estilos simulando UML
    classDef actor fill:#eceff1,stroke:#455a64,stroke-width:2px
    classDef usecase fill:#e3f2fd,stroke:#1565c0,stroke-width:2px,shape:ellipse

    %% Actores
    Admin(("👤 Administrador")):::actor
    Operador(("🧑‍💻 Usuario")):::actor
    Sistema(("⚙️ Motor de Seguridad")):::actor

    %% Frontera del Sistema
    subgraph "ERP - Gestión de Accesos y Seguridad"
        
        %% Casos de Uso: Configuración (Admin)
        CrearGrupo(["Registrar Grupo de Usuario"]):::usecase
        CfgMenu(["Configurar Permisos de Interfaz (Menú)"]):::usecase
        CfgTabla(["Configurar Permisos Transaccionales (Tablas)"]):::usecase

        %% Casos de Uso: Operación (Usuario)
        Navegar(["Navegar por los Módulos (Menú)"]):::usecase
        GestionarDatos(["Ejecutar Transacciones (ej. Guardar Factura)"]):::usecase
        
        %% Casos de Uso: Reglas (Sistema)
        ValidarVisibilidad(["Validar Permiso de Solo Lectura ('VER')"]):::usecase
        ValidarAccion(["Validar Privilegios CRUD (Agregar, Editar, etc)"]):::usecase
    end

    %% Conexiones Administrador
    Admin ---> CrearGrupo
    Admin ---> CfgMenu
    Admin ---> CfgTabla

    %% Conexiones Operador
    Operador ---> Navegar
    Operador ---> GestionarDatos

    %% Conexiones Sistema (Actor Secundario)
    ValidarVisibilidad <--- Sistema
    ValidarAccion <--- Sistema

    %% Relaciones UML (Includes)
    Navegar -. "<<include>>" .-> ValidarVisibilidad
    GestionarDatos -. "<<include>>" .-> ValidarAccion
```

### Explicación Funcional (Lógica de Negocio)

1. **Abstracción del Tecnicismo (El "Qué", no el "Cómo"):**
   En este diagrama los tecnicismos y llaves foráneas se abstraen en Casos de Uso comprensibles: **"Configurar Permisos de Interfaz"** y **"Configurar Permisos Transaccionales"**. Representan dos capas de configuración separadas pero complementarias dentro del flujo del sistema.

2. **La Separación del Menú vs. La Transacción:**
   La seguridad funciona bajo el principio de que la visibilidad no garantiza la operatividad. Esto se representa mediante las dependencias `<<include>>`:
   * Cuando el Operador intenta **Navegar por los Módulos**, el caso de uso incluye la obligación de **Validar el Permiso de 'VER'**.
   * Cuando el Operador intenta **Ejecutar una Transacción**, ocurre un escenario totalmente independiente que incluye **Validar Privilegios CRUD**. Si el usuario tiene acceso a la pantalla pero no tiene permiso de "Agregar" a nivel base de datos, el Motor de Seguridad bloquea la operación de guardado.

3. **Inclusión del Motor de Seguridad como "Actor Secundario":**
   En este modelo, el "Motor de Seguridad" es un actor automatizado del sistema. El Operador hace la petición (ver o guardar), pero es el Motor el que ejecuta internamente el Caso de Uso de "Validación", basándose estricta y automáticamente en las reglas que configuró el Administrador previamente.

## Diagrama de Secuencia: Operación Transaccional vs. Rendimiento

A continuación se modela el flujo lógico en el tiempo (*Sequence Diagram*) para una operación transaccional común (ej. Guardar una Factura). 

Una característica arquitectónica vital de este ERP es que **los permisos no se consultan a la base de datos en tiempo real por cada clic**. En su lugar, el sistema descarga todos los privilegios durante el proceso de arranque (Login) y los almacena en un **JSON en una variable de sesión local**. Esto garantiza validaciones instantáneas y evita sobrecargar el servidor de base de datos.

```mermaid
sequenceDiagram
    autonumber
    actor U as Operador
    participant UI as Pantalla Factura
    participant CS as Motor Seguridad
    participant MEM as JSON Sesión Local (Variable)
    participant DB as Base de Datos

    U->>UI: Clic en botón "Guardar Factura"
    UI->>CS: solicitarGuardado(datosFactura)
    
    %% Lectura en memoria (0 latencia)
    CS->>MEM: leerNodosJSON(tabla="Facturas")
    Note over CS,MEM: Se evita peticionar al servidor.<br/>Validación local e inmediata.
    MEM-->>CS: retorna permisos (VER=true, AGR=true)
    
    %% Fragmento UML condicional
    alt Permiso AGR == true
        CS->>DB: ejecutarInsert(datosFactura)
        DB-->>CS: OK
        CS-->>UI: confirmaciónGuardado()
        UI-->>U: Muestra "Factura guardada con éxito"
    else Permiso AGR == false
        CS-->>UI: abortarOperacion(Error)
        UI-->>U: Muestra "Error: No tienes permisos para Agregar"
    end
```

### Justificación Técnica
*   **Aislamiento de Carga:** Si el ERP tiene cientos de usuarios operando concurrentemente, consultar la tabla de permisos `001_PRM` antes de cada escritura crearía un cuello de botella masivo.
*   **Caché en Memoria:** Al aislar las reglas de negocio de la seguridad en el JSON local durante el arranque, el motor de seguridad actúa como un guardián de latencia cero.
*   *Nota:* El proceso detallado de cómo, cuándo y de dónde se extraen los datos para construir este JSON en memoria pertenece de forma exclusiva a la arquitectura del **Proceso de Inicialización de Sesión**, el cual se ejecuta una única vez durante el Login.

## Diagrama de Actividad: Algoritmo de Seguridad

A diferencia del proceso de arranque de sesión, este diagrama ilustra estricta y puramente **el algoritmo de toma de decisiones del Motor de Seguridad** cada vez que se requiere validar un permiso. 

Aquí se evidencia el comportamiento *polimórfico* del motor: la evaluación cambia lógicamente dependiendo de si lo que se está validando es un Menú (`004_OPC_MEN`) o una Acción Transaccional sobre datos (`004_CAT_TAB`).

```mermaid
flowchart TD
    %% Estilos UML estrictos
    classDef startEnd fill:#000,stroke:#000,color:#fff
    classDef action fill:#fff3e0,stroke:#ff9800,stroke-width:2px
    classDef decision fill:#e1bee7,stroke:#8e24aa,stroke-width:2px

    %% Nodos Inicial y Finales (Formato Seguro)
    Start(("Inicio: Llamada a Seguridad")):::startEnd
    EndOK(("Fin: Autorizado o Visible")):::startEnd
    EndErr(("Fin: Denegado u Oculto")):::startEnd

    %% Acciones iniciales
    A["Recibir Peticion de Validacion"]:::action
    B["Leer Variable Local JSON de Sesion"]:::action
    
    %% Rombo de Decision (Enrutador)
    C{"¿Que Tipo de Objeto (TIP_OBJ) se esta validando?"}:::decision
    
    %% Rama 1: Interfaz / Menu
    D_Menu["Buscar ID_MENU en JSON"]:::action
    E_Menu{"¿El nodo existe y el flag VER es true?"}:::decision
    
    %% Rama 2: Accion Transaccional / Tabla
    D_Tabla["Buscar ID_TBL en JSON"]:::action
    E_Tabla{"¿El nodo existe y la Accion solicitada es true?"}:::decision

    %% Flujos de Control UML (Flechas)
    Start --> A
    A --> B
    B --> C
    
    %% Bifurcacion Polimorfica
    C -->|Es una Interfaz o Menu| D_Menu
    C -->|Es Transaccion de Tabla| D_Tabla
    
    %% Evaluacion Menu
    D_Menu --> E_Menu
    E_Menu -->|Si| EndOK
    E_Menu -->|No| EndErr
    
    %% Evaluacion Tabla
    D_Tabla --> E_Tabla
    E_Tabla -->|Si| EndOK
    E_Tabla -->|No| EndErr
```

### Explicación del Algoritmo
1. **Aislamiento de Dominio:** Este diagrama demuestra que la validación de seguridad es completamente independiente del resto del ERP. Su única responsabilidad es recibir un ID, leer un JSON y devolver un `Sí` o `No`.
2. **Rutas Lógicas (TIP_OBJ):** El nodo de decisión central (`C`) es el núcleo del motor. Dictamina si la validación debe comportarse como un filtro visual (para pintar u ocultar botones en la UI) o como un cerrojo transaccional (para prevenir un INSERT o UPDATE no autorizado en la base de datos).
