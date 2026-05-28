# Arquitectura UML: Módulo Multiempresa (Multi-Tenant)

Este documento detalla el diseño arquitectónico del motor **Multi-Tenant (Multiempresa)** dentro de DATTA ERP. A diferencia del control de permisos, esta arquitectura dicta el contexto y el aislamiento absoluto de los datos en un entorno compartido.

El objetivo es garantizar que una única instancia de base de datos aloje de manera segura la información de múltiples entidades legales, asegurando que los usuarios operen exclusivamente sobre los datos autorizados en tiempo real.

```mermaid
classDiagram
    direction LR

    %% Capa de Seguridad y Acceso
    class USU {
        <<Maestro - Usuarios (001_USU)>>
        +Numérico ID
        +Alfa_256 NAME
        +Alfa_256 PSW_ACC
        +Numérico ENT_USU
    }

    class USU_EMP {
        <<Pivote - Autorización (001_USU_EMP)>>
        +Numérico ID
        +Numérico USU
        +Numérico EMP
        +Numérico CRD_USR
    }

    %% El Núcleo Multi-Tenant
    class EMP {
        <<Tenant - Empresas (001_EMP)>>
        +Numérico ID
        +Alfa_128 NAME
        +Numérico ENT
        +Booleano ES_EMP
        +Alfa_256 EMP_PAD
        +Objeto_Texto CONF_SIS
        +Booleano ACT
    }

    %% Representación resumida de la Operación
    class TABLAS_TRANSACCIONALES {
        <<Abstracción - Documentos/Movimientos>>
        +Numérico EMP (FK de Aislamiento)
        ... [CFDI, Pólizas, Asientos, etc.]
    }

    %% Relaciones Multiempresa
    USU "1" -- "0..*" USU_EMP : Tiene acceso a
    EMP "1" -- "0..*" USU_EMP : Autoriza a
    EMP "1" *-- "0..*" TABLAS_TRANSACCIONALES : Segmenta / Es dueña de

    %% Relación Jerárquica de Sucursales (Relación Reflexiva)
    EMP "1" o-- "0..*" EMP : Es matriz de (EMP_PAD)
```

## Explicación Arquitectónica (Semántica UML)

1. **Gestión de Acceso a Tenants (`001_USU_EMP`)**
   El acceso a una empresa es una relación de muchos-a-muchos. Esta lógica de autorización se almacena de forma independiente a los permisos en la tabla puente `001_USU_EMP`, permitiendo que un mismo usuario pueda tener "puntos de vista" distintos sobre diferentes razones sociales.

2. **Aislamiento de Datos por Composición (`*--`)**
   Toda tabla operativa (Facturas, Inventarios, Pólizas) hereda obligatoriamente el ID de la Empresa (`EMP`). En UML, esta composición indica que el dato no tiene existencia fuera de su Tenant, garantizando que el sistema segregue físicamente los registros mediante filtros de índice.

3. **Jerarquía Multi-Nivel (`EMP_PAD`)**
   La relación reflexiva (`o-- hacia sí misma`) permite modelar estructuras de Holdings o Consorcios, donde una Empresa Matriz puede contener múltiples empresas hijas o sucursales, manteniendo la integridad de la herencia de datos legales.

---

## Diagrama de Casos de Uso: Gestión y Contexto

A nivel de negocio, operar un sistema Multi-Tenant requiere flujos de trabajo específicos para navegar entre empresas y consolidar información sin cerrar la sesión activa.

```mermaid
flowchart LR
    %% Estilos UML corregidos para modo oscuro (se añade 'color')
    classDef actor fill:#eceff1,stroke:#455a64,stroke-width:2px,color:#1c313a
    classDef usecase fill:#e3f2fd,stroke:#1565c0,stroke-width:2px,shape:ellipse,color:#0d47a1

    %% Actores Externos Reales
    Admin(("👤 Administrador")):::actor
    Operador(("🧑‍💻 Usuario Operador")):::actor

    %% Límite del Sistema (con ID asignado para poder estilizarlo)
    subgraph sistema ["DATTA ERP - Gestión de Contexto Multiempresa"]

        %% Casos de Uso del Administrador
        Asignar(["Configurar Accesos a Empresas y Sucursales"]):::usecase

        %% Casos de Uso del Operador
        Alternar(["Alternar Empresa Activa en Interfaz"]):::usecase
        OperarCruzado(["Emitir Transacción para Empresa No Activa"]):::usecase
        ConsultarGlobal(["Consultar Historial Consolidado (Múltiples Empresas)"]):::usecase
    end

    %% Estilo adaptativo para el recuadro del sistema (sin fondo fijo)
    style sistema fill:none,stroke:#78909c,stroke-width:2px,stroke-dasharray: 5 5

    %% Interacciones
    Admin ---> Asignar

    Operador ---> Alternar
    Operador ---> OperarCruzado
    Operador ---> ConsultarGlobal
```

### Lógica de Negocio Multi-Tenant

- **Abstracción del Contexto:** El sistema permite que el operador tenga una "Empresa por Defecto" pero mantenga la capacidad de realizar transacciones cruzadas para otras empresas autorizadas sin cambiar su sesión global.
- **Seguridad de Capa Local:** La visibilidad de las empresas autorizadas se descarga una sola vez al inicio en el array `catalogo_EMP_AUTORIZADAS`, eliminando consultas redundantes al servidor durante la navegación.

---

## Diagrama de Secuencia: Operación Transaccional Multiempresa

Este diagrama modela el flujo técnico para una operación cruzada, demostrando cómo el sistema valida la seguridad en RAM antes de impactar la base de datos.

```mermaid
sequenceDiagram
    autonumber

    %% Definición de Participantes (Líneas de Vida)
    actor Luis as Vendedor: Luis
    participant UI as Interfaz (Módulo Facturas)
    participant RAM as Variable Global RAM ($SES_DAT)
    participant Core as Backend (Procesos Velneo 3er Plano)
    participant DB as Motor vServer (Base de Datos)

    %% FASE 1: VISUALIZACIÓN OMNIPRESENTE
    Note over Luis, DB: FASE 1: Visualización Cruzada Consolidada
    Luis ->> UI: Entra al "Listado de Facturas"
    activate UI
    UI ->> RAM: Leer catalogo_EMP_AUTORIZADAS
    RAM -->> UI: Retorna Array de IDs Permitidos (Ej. [1, 2, 3])

    UI ->> Core: Disparar Búsqueda (IDs_Empresas: [1, 2, 3])
    activate Core
    Core ->> DB: Resolver por Índice (IDX_EMP)
    activate DB
    DB -->> Core: Retorna Registros de A, B y C
    deactivate DB
    Core -->> UI: Retorna JSON consolidado
    deactivate Core
    UI ->> Luis: Renderiza Grid con historial multiempresa
    deactivate UI

    %% FASE 2: CREACIÓN Y CONMUTACIÓN EN CALIENTE
    Note over Luis, DB: FASE 2: Creación y Conmutación de Contexto
    Luis ->> UI: Clic en botón "Nueva Factura"
    activate UI
    UI ->> RAM: Leer globalVar_EMP_ACTUAL (ID: 1)
    RAM -->> UI: Retorna Empresa A
    UI ->> UI: Auto-completa Emisor con Empresa A

    Luis ->> UI: Cambia selector manual a Empresa B (ID: 2)
    Luis ->> UI: Clic en botón "Guardar y Timbrar"

    %% Validación Perimetral de Seguridad en Memoria
    UI ->> RAM: ¿ID_Empresa: 2 existe en catalogo_EMP_AUTORIZADAS?
    RAM -->> UI: Retorna TRUE (Operación lícita)

    alt ID Autorizado Exitosamente (Camino Feliz)
        UI ->> Core: Lanzar Proceso (Payload + EMP_DESTINO = 2)
        activate Core
        Note over Core: El Proceso inyecta el ID recibido<br/>antes de crear el registro físico.
        Core ->> Core: Firmar XML usando CSD de Empresa B
        Core ->> DB: Alta de Ficha en 003_CFDI (ID 2)
        activate DB
        DB -->> Core: OK
        deactivate DB
        Core -->> UI: Retorno Exitoso
        deactivate Core
        UI ->> Luis: Notifica Guardado en Empresa B
    else ID No Autorizado
        UI ->> Luis: Alerta: "Acceso denegado"
    end
    deactivate UI
```

### Arquitectura Técnica en el Tiempo

1. **Visión Omnipresente:** El ERP permite cargar plurales de múltiples empresas simultáneamente mediante el paso de arrays al backend, optimizando el rendimiento mediante el uso de índices de base de datos.
2. **Validación Fail-Fast en RAM:** La seguridad perimetral se ejecuta en la UI comparando el destino contra el catálogo autorizado en memoria local, eliminando latencia y denegando manipulaciones antes de llegar al servidor.

---

## Diagrama de Actividad: Algoritmo de Transacción Defensiva

El diagrama de actividad representa el **algoritmo matemático** que sigue la interfaz para blindar la integridad del Tenant durante una operación de escritura.

```mermaid
flowchart TD
    %% Estilos UML optimizados para modo claro y oscuro
    classDef startEnd fill:#263238,stroke:#cfd8dc,stroke-width:2px,color:#fff
    classDef action fill:#fff3e0,stroke:#ff9800,stroke-width:2px,color:#4e342e
    classDef decision fill:#e1bee7,stroke:#8e24aa,stroke-width:2px,color:#4a148c

    %% Nodos Inicial y Finales
    Start(("Inicio: Intento de Transacción Cruzada")):::startEnd
    EndOk(("Fin: Alta de Ficha Exitosa")):::startEnd
    EndErr(("Fin: Operación Abortada Localmente")):::startEnd

    %% Acciones de Interfaz
    A["Capturar ID_EMP del Formulario"]:::action
    B["Leer catalogo_EMP_AUTORIZADAS (RAM)"]:::action

    %% Rombo de Decisión (Filtro de Seguridad)
    D{"¿El ID seleccionado está<br/>incluido en el Array?"}:::decision

    %% Rutas
    Err["Bloquear UI y Mostrar Alerta"]:::action
    Ok1["Inyectar ID_EMP y lanzar Proceso"]:::action
    Ok2["vServer: Alta de Ficha (Asignar Tenant)"]:::action

    %% Flujo
    Start --> A
    A --> B
    B --> D

    D -->|"No (Acceso Denegado)"| Err
    Err --> EndErr

    D -->|"Sí (Autorizado)"| Ok1
    Ok1 --> Ok2
    Ok2 --> EndOk
```

### Análisis del Algoritmo

1. **Validación en Capa de Cliente:** Ahorra latencia de red y ciclos de CPU en el servidor al denegar intentos de inyección de datos desde la interfaz de usuario.
2. **Desacoplamiento de Contexto:** Permite que un usuario emita documentos para cualquier empresa autorizada sin necesidad de forzar un cambio de sesión global, aumentando la productividad operativa.
3. **Persistencia Multi-Tenant:** La instrucción final garantiza que el puntero maestro de la base de datos se asigne correctamente, manteniendo el aislamiento absoluto de los registros.
