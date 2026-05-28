## Diagrama de Clases UML: Estructura del CFDI (Módulo 003)

```mermaid
classDiagram
    direction TB

    class `003_CFDI` {
        <<Maestro / Cabecera>>
        +ID : Numérico
        +UUID : Alfa
        +SER : Alfa
        +FOL : Alfa
        +FECH_EMI : Fecha
        +FRM_PAG : Numérico
    }

    class `003_PAR` {
        <<Participantes: Nodo Emisor y Receptor>>
        +ID : Numérico
        +CFDI : Numérico
        +ENT : Numérico
        +ROL : Alfa
        +REG_FIS : Alfa
        +USO_CFDI : Alfa
        +CP_PAR : Alfa
    }

    class `003_CFDI_CON` {
        <<Nodo Conceptos (Partidas)>>
        +ID : Numérico
        +CFDI_CON : Numérico
        +COD_SAT : Alfa
        +CANT : Numérico
        +CLV_UNI_SAT : Alfa
        +NOM_DES : Alfa
        +VAL_UNI : Numérico
        +IMPO : Numérico
        +DESC : Numérico
    }

    class `003_IMP` {
        <<Sumatización Nodo Impuestos>>
        +ID : Numérico
        +CFDI_IMP : Numérico
        +TOT_TRAS : Numérico
        +CFDI_RET_TRAS : Numérico
    }

    class `003_IMP_TRSL` {
        <<Nodo Traslados (Detalle)>>
        +ID : Numérico
        +CFDI_PAD : Numérico
        +CLV_IMP_SAT : Alfa
        +TIP_FAC_SAT : Alfa
        +TAS_CUO : Numérico
        +NUM_BASE : Numérico
        +NUM_IMP : Numérico
    }

    class `003_IMP_RET` {
        <<Nodo Retenciones (Detalle)>>
        +ID : Numérico
        +CFDI_PAD : Numérico
        +CLV_IMP_SAT : Alfa
        +TIP_FAC_SAT : Alfa
        +TAS_CUO : Numérico
        +NUM_BASE : Numérico
        +NUM_IMP : Numérico
    }

    class `003_CPLM` {
        <<Complementos (Extensibilidad)>>
        +ID : Numérico
        +CFDI : Numérico
        +TIP : Alfa
        +JSN_DAT : Objeto Texto
    }

    class `003_CFDIS_REL` {
        <<Nodo CfdiRelacionados>>
        +ID : Numérico
        +CFDI_PAD : Numérico
        +UUID_REL : Alfa
        +TIP : Alfa
    }

    %% Relaciones UML: Composición (*--)
    `003_CFDI` "1" *-- "1..2" `003_PAR` : Participa
    `003_CFDI` "1" *-- "1..*" `003_CFDI_CON` : Contiene
    `003_CFDI` "1" *-- "0..*" `003_CPLM` : Anexa
    `003_CFDI` "1" *-- "0..*" `003_CFDIS_REL` : Relaciona

    %% Relaciones UML: Impuestos
    `003_CFDI` "1" *-- "0..1" `003_IMP` : Resume Totales
    `003_CFDI` "1" *-- "0..*" `003_IMP_TRSL` : Traslada
    `003_CFDI` "1" *-- "0..*" `003_IMP_RET` : Retiene
```

**Análisis Técnico Fiscal del Diagrama:**

- **Composición (Rombo negro \*--)**: En UML, esto indica que las tablas hijas "mueren" si eliminas la cabecera. Es exacto al XML del SAT: un nodo Concepto o Traslado no puede existir flotando solo, pertenece intrínsecamente a un Comprobante.

- **Participantes (003_PAR)**: Aquí se guarda la "fotografía" del RFC y Régimen Fiscal en el momento exacto en que se emitió el CFDI. Esto es vital para el Anexo 20 (versión 4.0), donde el Código Postal (CP_PAR) y el Régimen Fiscal (REG_FIS) del receptor deben coincidir exactamente con la Constancia de Situación Fiscal.

- **Desglose granular de Impuestos (003_IMP_TRSL y 003_IMP_RET)**: El estándar del SAT obliga a desglosar los impuestos por cada tasa y tipo (ej. separar el IVA al 16% del IVA al 8%). Por eso, no se guarda un simple campo "Total IVA" en la cabecera, sino que cada impuesto tiene su propio registro en estas tablas hijas para armar los nodos Traslados y Retenciones.

- **Complementos en JSON (003_CPLM)**: En lugar de hacer una tabla inmensa para cada complemento del SAT (Pagos, Carta Porte, Nómina), tu arquitectura es brillante al utilizar un campo JSN_DAT (Objeto Texto). Esto permite inyectar cualquier estructura técnica del Anexo 20 sin modificar el esquema de base de datos.

## Diagrama de Casos de Uso UML: Módulo CFDI (DS-QS)

```mermaid
flowchart LR
    %% Actores
    Usuario((👤 Usuario\nContable))
    NodeJS((🤖 Microservicio\nNode.js / IA))
    SAT((🏛️ API del SAT))

    %% Límite del Sistema (Frontera UML)
    subgraph Sistema_ERP_DS_QS ["📦 Módulo CFDI (Base de Datos Velneo)"]
        direction TB

        %% Casos de Uso Principales
        UC1([Descargar CFDIs Masivamente])
        UC2([Procesar Estructura XML del CFDI])
        UC3([Clasificar Conceptos con IA])
        UC4([Consultar CFDI y Totales])
        UC5([Gestionar Complementos Anexos])
        UC6([Asignar CFDI Relacionado])

        %% Casos de Uso Incluidos (Obligatorios según Anexo 20)
        UC_PAR([Extraer Participantes y Régimen])
        UC_CON([Extraer Conceptos y Claves SAT])
        UC_IMP([Desglosar Impuestos Trasladados y Retenidos])
    end

    %% Relaciones de Actores Externos
    NodeJS --- UC1
    UC1 --- SAT
    NodeJS --- UC2
    NodeJS --- UC3

    %% Relaciones del Usuario
    Usuario --- UC4
    Usuario --- UC5
    Usuario --- UC6

    %% Relaciones UML estandarizadas (include / extend)
    UC2 -. "<<include>>" .-> UC_PAR
    UC2 -. "<<include>>" .-> UC_CON
    UC2 -. "<<include>>" .-> UC_IMP

    %% La IA es un proceso que extiende el procesamiento básico del XML
    UC3 -. "<<extend>>" .-> UC2

    %% Complementos y Relaciones son opcionales para un CFDI (extend)
    UC5 -. "<<extend>>" .-> UC2
    UC6 -. "<<extend>>" .-> UC2

    %% --- SECCIÓN DE ESTILOS ADAPTATIVOS MULTI-TEMA ---
    %% Forzamos 'color' oscuro en las clases para blindar el texto ante el modo oscuro
    classDef actor fill:#f5f5f5,stroke:#333333,stroke-width:2px,color:#212121;
    classDef usecase fill:#e1f5fe,stroke:#01579b,stroke-width:1px,color:#00365c;

    class Usuario,NodeJS,SAT actor;
    class UC1,UC2,UC3,UC4,UC5,UC6,UC_PAR,UC_CON,UC_IMP usecase;

    %% Contenedor transparente con borde discontinuo gris neutro para resaltar en ambos temas
    style Sistema_ERP_DS_QS fill:none,stroke:#78909c,stroke-width:2px,stroke-dasharray: 5 5
```

## Análisis Fiscal y Arquitectónico del Diagrama UML

- **Relaciones <<include>> (Lo Obligatorio del SAT):** En UML, un <<include>> significa que un caso de uso no puede existir sin ejecutar el otro. Físicamente en tu ERP, cuando Node.js ejecuta "Procesar Estructura XML", está obligado a separar la información en tus tablas maestras para cumplir el Anexo 20:

- **Extraer Participantes:** Llena la tabla 003_PAR con el RFC y Código Postal.

- **Extraer Conceptos:** Llena la tabla 003_CFDI_CON para no perder la clave del producto y cantidad.

- **Desglosar Impuestos:** Llena las tablas 003_IMP_TRSL y 003_IMP_RET para separar el IVA del ISR según la regla fiscal.

- **Relaciones <<extend>> (Lo Opcional o Condicional):** Un <<extend>> significa que es un comportamiento que puede ocurrir bajo ciertas condiciones, pero no siempre.

- **Clasificar Conceptos con IA:** La IA actúa sobre el XML extraído para proponer un asiento contable.

- **Gestionar Complementos:** Se ejecuta solo si el CFDI tiene nodos extra (ej. Pagos o Nómina), inyectando el JSON en tu tabla 003_CPLM.

- **Asignar CFDI Relacionado:** Se ejecuta solo si el documento reemplaza o está vinculado a un UUID anterior, alimentando la tabla 003_CFDIS_REL.

- **Los Actores Separados:** Puedes notar que el Usuario Contable no participa directamente en la descarga o procesamiento. Su caso de uso principal se centra en Consultar y Gestionar excepciones, cumpliendo con la filosofía de "automatización contable total" de tu fase 1.

## Diagrama de Actividad UML: Procesamiento y Extracción de CFDI (DS-QS)

```mermaid
stateDiagram-v2
    direction TB

    %% Nodo de Inicio UML
    [*] --> Descargar_XML_SAT

    %% Actividades Secuenciales (Lectura Obligatoria Anexo 20)
    Descargar_XML_SAT --> Parsear_Documento_XML
    Parsear_Documento_XML --> Extraer_Cabecera_003_CFDI
    Extraer_Cabecera_003_CFDI --> Extraer_Participantes_003_PAR
    Extraer_Participantes_003_PAR --> Extraer_Conceptos_003_CFDI_CON
    Extraer_Conceptos_003_CFDI_CON --> Extraer_Impuestos_Desglosados

    %% Barra de Bifurcación (Fork) - Procesamiento Paralelo UML
    state Fork_Nodos_Opcionales <<fork>>
    Extraer_Impuestos_Desglosados --> Fork_Nodos_Opcionales

    %% Hilo 1: Evaluación de Complementos
    Fork_Nodos_Opcionales --> Evaluar_Complementos
    state Condicion_CPLM <<choice>>
    Evaluar_Complementos --> Condicion_CPLM
    Condicion_CPLM --> Convertir_a_JSON_003_CPLM : [Existe Complemento]
    Condicion_CPLM --> Join_Sincronizacion : [No Existe]
    Convertir_a_JSON_003_CPLM --> Join_Sincronizacion

    %% Hilo 2: Evaluación de CFDI Relacionados
    Fork_Nodos_Opcionales --> Evaluar_Relacionados
    state Condicion_REL <<choice>>
    Evaluar_Relacionados --> Condicion_REL
    Condicion_REL --> Extraer_UUIDs_003_CFDIS_REL : [Existen Relacionados]
    Condicion_REL --> Join_Sincronizacion : [No Existen]
    Extraer_UUIDs_003_CFDIS_REL --> Join_Sincronizacion

    %% Barra de Sincronización (Join) - Fin del paralelismo UML
    state Join_Sincronizacion <<join>>

    %% Transición a Inteligencia Artificial e Inserción
    Join_Sincronizacion --> Clasificacion_IA_NodeJS
    Clasificacion_IA_NodeJS --> Enviar_JSON_a_Velneo
    Enviar_JSON_a_Velneo --> Insertar_Transaccion_Velneo

    %% Nodo de Fin UML
    Insertar_Transaccion_Velneo --> [*]
```

## Análisis Técnico Arquitectónico del Diagrama

- **El Inicio Secuencial (La Regla Fiscal):** En UML, la flecha directa entre actividades implica que no puedes pasar al siguiente paso sin terminar el anterior. Esto obedece al Anexo 20: No puedes extraer los impuestos (003_IMP_TRSL) si no has validado primero la cabecera y el concepto.

- **<<fork>> y <<join>> (Las barras negras gruesas horizontales):**

- **Justificación de arquitectura:** Una vez extraídos los nodos obligatorios, tu código de Node.js no tiene por qué bloquearse para leer lo demás. Puede usar procesamiento paralelo (o asíncrono).

- **La barra fork** divide el flujo para que el motor busque Complementos y CFDI Relacionados de forma simultánea.

- **La barra join** obliga al sistema a esperar a que ambas validaciones opcionales terminen antes de pasar a la Inteligencia Artificial.

- **<<choice>> (Los rombos condicionales):**

- **En UML, los rombos evalúan las "condiciones de guarda" expresadas entre corchetes [ ]. Esto mapea perfectamente la lógica de programación fiscal:**

- **[Existe Complemento]** -> Construye el objeto texto (JSON) y prepara el alta para la tabla 003_CPLM.

- **[Existen Relacionados]** -> Extrae los UUID previos para insertarlos en 003_CFDIS_REL.

- **El Límite del Sistema:**

**La actividad Enviar_JSON_a_Velneo representa la frontera (API REST) donde termina el trabajo del microservicio Sidecar de Node.js y comienza el trabajo del Core en Velneo V7 (Insertar_Transaccion_Velneo).**

## Diagrama de Secuencia UML: Orquestación del CFDI 4.0 a Contabilidad

```mermaid
sequenceDiagram
    autonumber

    participant API as ⚙️ Controlador Velneo (API REST)
    participant T_CFDI as 🗂️ Tabla: 003_CFDI (Maestro)
    participant T_PAR as 🗂️ Tabla: 003_PAR (Participantes)
    participant T_CON as 🗂️ Tabla: 003_CFDI_CON (Conceptos)
    participant T_IMP as 🗂️ Tabla: Módulo 003_IMP (Impuestos)
    participant T_OPC as 🗂️ Tablas Opcionales (CPLM/REL)

    API->>API: 🔒 Iniciar Transacción de Base de Datos (ACID)

    %% INSERCIÓN DEL MAESTRO
    Note over API, T_CFDI: 1. Creación del Registro Padre (Indispensable)
    API->>+T_CFDI: Insertar (UUID, SER, FOL, FECH_EMI, FRM_PAG)
    T_CFDI-->>-API: Retorna ID_CFDI_Creado

    %% INSERCIÓN DE PARTICIPANTES
    Note over API, T_PAR: 2. Resolución de Identidad
    loop Por cada Emisor y Receptor
        API->>+T_PAR: Insertar (CFDI = ID_CFDI_Creado, ENT, ROL, REG_FIS, CP_PAR)
        T_PAR-->>-API: OK
    end

    %% INSERCIÓN DE PARTIDAS
    Note over API, T_CON: 3. Detalle Comercial
    loop Por cada Partida (Concepto) del XML
        API->>+T_CON: Insertar (CFDI_CON = ID_CFDI_Creado, COD_SAT, CANT, VAL_UNI, IMPO)
        T_CON-->>-API: OK
    end

    %% INSERCIÓN DE IMPUESTOS
    Note over API, T_IMP: 4. Sumatización y Desglose Fiscal
    API->>+T_IMP: Insertar 003_IMP (CFDI_IMP = ID_CFDI_Creado, TOT_TRAS, CFDI_RET_TRAS)
    T_IMP-->>-API: OK

    loop Por cada Impuesto Desglosado (Traslados y Retenciones)
        API->>+T_IMP: Insertar 003_IMP_TRSL / 003_IMP_RET (CFDI_PAD = ID_CFDI_Creado, CLV_IMP_SAT, TAS_CUO, NUM_IMP)
        T_IMP-->>-API: OK
    end

    %% INSERCIÓN DE NODOS OPCIONALES
    Note over API, T_OPC: 5. Extensiones y Relaciones
    opt Si contiene Complemento (Ej. Pago/Nómina)
        API->>+T_OPC: Insertar 003_CPLM (CFDI = ID_CFDI_Creado, TIP, JSN_DAT)
        T_OPC-->>-API: OK
    end

    opt Si contiene CFDI Relacionados
        API->>+T_OPC: Insertar 003_CFDIS_REL (CFDI_PAD = ID_CFDI_Creado, UUID_REL, TIP)
        T_OPC-->>-API: OK
    end

    API->>API: ✅ Commit Transacción (Escritura final en Disco)
```

## Explicación Estricta de la Persistencia en Velneo

Este diagrama explica por qué la estructura de tus tablas está diseñada con campos de "Enlace a Maestro" (como CFDI_CON o CFDI_PAD) y cómo debe programarse el guardado:

- **La dependencia del ID Maestro (Paso 2):** En UML, la tabla 003_CFDI se atiende primero. Velneo debe insertar la cabecera y generar el ID_CFDI_Creado (el identificador numérico interno). Sin este número, todas las demás tablas fallarían porque no tendrían a quién "apuntar".

- **Los Bucles loop (Pasos 3, 4 y 6):** En el XML del SAT pueden venir múltiples conceptos y múltiples participantes. Velneo debe iterar (hacer un bucle) e insertar un registro en 003_PAR por cada participante, y un registro en 003_CFDI_CON por cada partida, pasándoles a todas el mismo ID_CFDI_Creado que obtuviste en el paso 2.

- **El Desglose Impositivo (Pasos 5 y 6):** Primero guardas los totales en la tabla de apoyo 003_IMP. Luego, debes hacer un bucle para insertar en 003_IMP_TRSL y 003_IMP_RET la tasa exacta (TAS_CUO) y el importe (NUM_IMP) de cada impuesto separado.

- **Transaccionalidad (Commit/Rollback):** Nota que todo está envuelto entre "Iniciar Transacción" y "Commit Transacción". Si por alguna razón la tabla de Conceptos (003_CFDI_CON) falla al guardarse, el controlador aborta (hace un Rollback) y no se guarda nada. Esto evita que queden cabeceras huérfanas sin conceptos o impuestos a medias en tu base de datos.
