# Arquitectura UML: Módulo Multiempresa (Multi-Tenant)

Este documento detalla el diseño arquitectónico del motor **Multi-Tenant (Multiempresa)** dentro de DATTA ERP. A diferencia del control de permisos (que dicta *qué acciones* puede realizar un usuario), esta arquitectura dicta **el contexto y el aislamiento de los datos**.

El objetivo principal de este módulo es garantizar que una única instancia de base de datos pueda alojar de manera segura la información de múltiples entidades legales (Empresas), asegurando que los usuarios únicamente visualicen y operen sobre los datos de la empresa en la que están "conectados" en ese momento.

## Diagrama de Clases: Estructura de Datos Multi-Tenant

El siguiente diagrama ilustra cómo se ligan los Usuarios con las Empresas, y cómo este diseño impone un filtro físico a las tablas transaccionales.

```mermaid
classDiagram
    %% Relación de asignación de acceso (Muchos a Muchos)
    001_USU "1" -- "0..*" 001_USU_EMP : Tiene acceso a
    001_EMP "1" -- "0..*" 001_USU_EMP : Accesible por
    
    %% Jerarquía Organizacional Interna
    001_EMP "1" *-- "0..*" 001_SUC : Contiene Sucursales
    
    %% Aislamiento de Datos (Ejemplo de Composición hacia el Tenant)
    001_EMP "1" *-- "0..*" TABLA_TRANSACCIONAL : Aísla registros de

    class 001_EMP {
        <<Maestro - Empresas (Tenants)>>
        +Numérico ID
        +Alfa_128 NAME
        +Numérico ENT
        +Booleano ACT
    }
    
    class 001_SUC {
        <<Detalle - Sucursales>>
        +Numérico ID
        +Alfa_128 NAME
        +Numérico EMP
    }
    
    class 001_USU {
        <<Maestro - Usuarios>>
        +Numérico ID
        +Alfa_60 NAME
    }
    
    class 001_USU_EMP {
        <<Tabla Puente - Acceso a Tenants>>
        +Numérico USU
        +Numérico EMP
    }
    
    class TABLA_TRANSACCIONAL {
        <<Cualquier tabla operativa (Ej. Facturas)>>
        +Numérico ID
        +Numérico EMP (Llave del Tenant)
        +... Datos()
    }
```

### Explicación Arquitectónica (Semántica UML)

1. **Gestión de Acceso a Tenants (`001_USU_EMP`)**
   El acceso a una empresa es una relación de muchos-a-muchos. Un operador contable puede requerir acceso a 3 empresas distintas del corporativo, mientras que un vendedor solo a 1. Esta lógica de autorización se almacena de forma independiente a los permisos en la tabla puente `001_USU_EMP`.

2. **Aislamiento de Datos por Composición (`*-- TABLA_TRANSACCIONAL`)**
   En una arquitectura SaaS/Cloud verdadera, toda tabla transaccional o catálogo específico del negocio (Facturas, Clientes, Inventarios, Cuentas Bancarias) debe heredar obligatoriamente el ID de la Empresa (`EMP`). Esta relación de composición indica que el dato le pertenece exclusivamente a ese Tenant y, matemáticamente, no puede cruzarse con los datos de otra empresa.

3. **Sub-Partición Espacial (`001_SUC`)**
   Dentro del aislamiento maestro de la Empresa, existe un sub-nivel geográfico o lógico (`001_SUC`), permitiendo aislar la operación a nivel almacén o sucursal sin salir de la misma razón social.

---

## Diagrama de Casos de Uso: Gestión y Contexto

A nivel de negocio, operar un sistema Multi-Tenant requiere flujos de trabajo específicos para navegar entre las diferentes empresas sin tener que cerrar sesión.

```mermaid
flowchart LR
    classDef actor fill:#eceff1,stroke:#455a64,stroke-width:2px
    classDef usecase fill:#e3f2fd,stroke:#1565c0,stroke-width:2px,shape:ellipse

    Admin(("👤 Administrador")):::actor
    Operador(("🧑‍💻 Usuario Operador")):::actor
    Sistema(("⚙️ Motor Multi-Tenant")):::actor

    subgraph "DATTA ERP - Aislamiento Multiempresa"
        Asignar(["Otorgar Acceso a Empresas"]):::usecase
        Elegir(["Seleccionar Empresa (Cambio de Contexto)"]):::usecase
        Filtrar(["Aplicar Filtro Espacial (Aislamiento)"]):::usecase
    end

    %% Relaciones de los Actores
    Admin ---> Asignar
    Operador ---> Elegir
    Filtrar <--- Sistema
    
    %% Inclusión Obligatoria
    Elegir -. "<<include>>" .-> Filtrar
```

### Lógica de Negocio Multi-Tenant

1. **La Meta del Operador:** El operador no "filtra bases de datos"; su única meta de negocio es **"Seleccionar Empresa"** en la interfaz para comenzar a trabajar.
2. **La Responsabilidad del Sistema (`<<include>>`):** Cada vez que el operador elige una empresa diferente, el Motor del ERP está obligado silenciosamente a ejecutar el caso de uso **"Aplicar Filtro Espacial"**. Esto es lo que expulsa los datos de la Empresa A de la memoria de la interfaz, e inyecta los datos de la Empresa B, asegurando la integridad visual.

---

## Diagrama de Secuencia: Cambio de Contexto (Switch Tenant)

El siguiente diagrama detalla cómo se ejecuta técnicamente un "Cambio de Empresa" en caliente (en tiempo de ejecución). Al igual que en la seguridad de los permisos, **la variable de sesión** (`JSON_SES`) juega un rol fundamental para dictar el "Punto de Vista" actual de toda la aplicación.

```mermaid
sequenceDiagram
    autonumber
    actor U as Operador
    participant UI as Selector de Empresa (UI)
    participant CS as Controlador de Contexto
    participant MEM as JSON Sesión Local
    participant DB as Base de Datos

    U->>UI: Selecciona "Empresa B" en el menú
    activate UI
    
    UI->>CS: solicitarCambioContexto(ID_Empresa_B)
    activate CS
    
    %% Validación de Integridad
    CS->>DB: Validar acceso en 001_USU_EMP
    activate DB
    DB-->>CS: Acceso Concedido
    deactivate DB
    
    %% Mutación del Contexto Global
    CS->>MEM: Mutar( globalVar_CUR_EMP_ID = ID_Empresa_B )
    Note over CS,MEM: El "Tenant" activo acaba de cambiar.<br/>Las variables globales de la aplicación apuntan ahora a la nueva Empresa.
    
    %% Recarga del Sistema
    CS-->>UI: Ordenar Refresco Total de Interfaz
    deactivate CS
    
    %% El filtro cobra vida
    UI->>DB: Cargar Tablas (Aplicando WHERE EMP = globalVar_CUR_EMP_ID)
    activate DB
    DB-->>UI: Retorna SOLO registros de la Empresa B
    deactivate DB
    
    UI-->>U: Interfaz renderizada en el nuevo entorno
    deactivate UI
```

### Arquitectura Técnica en el Tiempo

1. **Validación Previa al Cambio:** El controlador de contexto (`CS`) siempre realiza una revalidación rápida (`001_USU_EMP`) antes de conceder el cambio, previniendo ataques de inyección de IDs o vulnerabilidades por sesiones de red.
2. **Mutación de la Persistencia Local:** Al sobrescribir la variable global `globalVar_CUR_EMP_ID` dentro del `JSON_SES`, se altera el núcleo del ERP. A partir de este nanosegundo, cualquier pantalla, reporte o proceso en segundo plano que se abra leerá esta variable y se comportará como si el ERP solo perteneciera a la "Empresa B".
3. **Aislamiento Implícito (El Filtro):** La interfaz (`UI`) asume un diseño defensivo. Al redibujarse, automáticamente adjunta el `globalVar_CUR_EMP_ID` a las condiciones de carga de plurales y listas, logrando el efecto Multi-Tenant de forma centralizada y sin necesidad de programar el filtro en cada botón individual de la aplicación.
