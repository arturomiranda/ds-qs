# Arquitectura UML: Módulo de Principales (Entidades y Relaciones)

A continuación se presenta el Diagrama de Clases modelado con la sintaxis de UML para representar el ecosistema de la tabla central `001_ENT` (Entidades) y su relación con Empresas, Direcciones, Teléfonos, Correos y Notas.

```mermaid
classDiagram
    EMP "1" *-- "0..*" ENT : Pertenece a (Multi-Tenant)
    EMP "0..1" o-- "1" ENT : Datos Legales y Fiscales
    ENT "1" o-- "0..*" USU : Es Operador (Credenciales)
    ENT "1" *-- "0..*" DIR : Tiene Direcciones
    ENT "1" *-- "0..*" TEL : Tiene Teléfonos
    ENT "1" *-- "0..*" EMA : Tiene Correos
    ENT "1" *-- "0..*" NOTA : Tiene Notas
    class EMP {
        <<Maestro - Empresas (001_EMP)>>
        +Numérico ID
        +Alfa_128 NAME
        +Numérico ENT
        +Booleano ES_EMP
        +Alfa_256 EMP_PAD
        +Objeto_Texto CONF_SIS
        +Booleano ACT
    }
    class ENT {
        <<Maestro - Entidades (001_ENT)>>
        +Numérico ID
        +Alfa_256 NAME
        +Numérico EMP
        +Numérico SUC
        +Alfa_13 RFC
        +Booleano ES_EMPR
        +Booleano ES_PRV
        +Booleano ES_CLT
        +Booleano ES_EMPL
        +Booleano ES_COM
        +Alfa_40 UUID_EX
        +Booleano OFF
    }
    class DIR {
        <<Detalle - Direcciones (001_DIR)>>
        +Numérico ID
        +Numérico ENT
        +Alfa_1 TIP_DIR
        +Alfa_254 CLLE
        +Alfa_20 NUM_EXT_DIR
        +Alfa_20 NUM_INT_DIR
        +Numérico COL_DIR
        +Alfa_5 CP_DIR
        +Numérico CIU_DIR
        +Alfa_3 EST_DIR
        +Alfa_3 PAIS_DIR
    }
    class TEL {
        <<Detalle - Teléfonos (001_TEL)>>
        +Numérico ID
        +Numérico ENT
        +Alfa_14 NUM
        +Alfa_1 TIP
    }
    class EMA {
        <<Detalle - Correos Electrónicos (001_EMA)>>
        +Numérico ID
        +Numérico ENT_EMA
        +Alfa_100 DIR_EMA
    }
    class NOTA {
        <<Detalle - Notas (001_NOTA)>>
        +Numérico ID
        +Numérico ENT
        +Alfa_256 CON
        +Fecha FECH
        +Numérico CRD_USR
        +Tiempo CRD_TIM
    }
    class USU {
        <<Maestro - Usuarios (001_USU)>>
        +Numérico ID
        +Alfa_60 NAME
        +Alfa_256 PSW_ACC
        +Numérico ENT
        +Booleano EXT
        +Booleano OFF
    }
```

## Explicación Arquitectónica (Semántica UML)

1. **Relación entre Empresas (`001_EMP`) y Entidades (`001_ENT`)**
   * **Composición por Aislamiento (`*--`):** La clase `EMP` compone a muchísimas `ENT`. A través de la llave foránea `EMP` dentro de `001_ENT`, el sistema dictamina a qué empresa o "tenant" pertenece cada cliente, proveedor o empleado.
   * **Agregación Identitaria (`o--`):** Simultáneamente, la tabla `001_EMP` utiliza una relación de uno-a-uno vinculándose a `001_ENT` mediante el campo `ENT`. Esto le permite a la empresa "heredar" toda su información fiscal y legal (su propio RFC, Razón Social, etc.) desde la central maestra, evitando redundancia de campos en la base de datos.

2. **La Entidad (`001_ENT`) como Súper-Clase Central**
   * Funciona como el pivote maestro del diseño. Centraliza a todas las personas físicas o morales (cuyo elemento unificador es el `RFC`) y, mediante atributos de estado (`ES_EMPR`, `ES_CLT`, `ES_PRV`, `ES_EMPL`), determina el rol o "polimorfismo" de la entidad dentro de las operaciones del ERP.

3. **Satélites de Contacto y CRM (Composiciones Estrictas `*--`)**
   * En UML, utilizamos el rombo relleno (`*--`) para representar una relación de **Composición** (Padre-Hijo fuerte). Esto significa que las direcciones, correos, teléfonos y notas no tienen independencia existencial sin su Entidad dueña:
     * **Direcciones (`001_DIR`):** Relación 1 a *N*. Permite guardar la dirección fiscal, almacenes o lugares de entrega mediante el discriminador `TIP_DIR`.
     * **Teléfonos (`001_TEL`):** Relación 1 a *N*. Resuelve la limitación de campos fijos y permite una colección ilimitada de teléfonos móviles o fijos mediante `TIP`.
     * **Correos (`001_EMA`):** Relación 1 a *N*. Habilita vincular múltiples cuentas de correo al sujeto de negocios, enlazando directamente al ID maestro a través de `ENT_EMA`.
     * **Notas (`001_NOTA`):** Relación 1 a *N*. Colección de bitácoras de texto que soportan las actividades CRM, todas apuntando a la Entidad base a través de `ENT`.

4. **Gestión de Identidad y Acceso (`001_USU`)**
   * **Agregación (`o--`):** La tabla `001_USU` (Usuarios) centraliza las credenciales de acceso al ERP. Mantiene una relación de Agregación con `001_ENT` mediante el campo `ENT`. Esto vincula la cuenta de usuario con una persona física o actor comercial del sistema, otorgándole identidad y datos al operador del software.
