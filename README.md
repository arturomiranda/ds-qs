# DS-QS: ERP Intelligent Suite (Phase 1) 🚀

**DS-QS** es un ERP de siguiente generación diseñado para la automatización contable total. La arquitectura se basa en la potencia de **Velneo** para la gestión de datos e interfaz, potenciada por un ecosistema de microservicios en **Node.js**.

## 🏗️ Arquitectura del Proyecto

* **Core & UI:** [Velneo](https://velneo.es/) (Base de datos objeto-relacional y vClient).
* **Service Layer:** [Node.js v22.21.1](https://nodejs.org/) (WSL / Ubuntu).
* **IA Engine:** Integración con modelos de lenguaje para clasificación contable automática.
* **Integración:** Protocolo REST/JSON entre Velneo y el microservicio de Node.js.

---

## 📌 Requerimientos: Fase 1 - Contabilidad Financiera

### 1. Núcleo Contable (Velneo V7)
Desarrollo de la estructura transaccional en la plataforma Velneo:
* **Maestros Contables:** Tablas de Entidades, Ejercicios Fiscales, Periodos y Catálogo de Cuentas (Estructura de Árbol).
* **Motor de Saldos:** Gestión de saldos acumulados y movimientos de pólizas en tiempo real.
* **Interfaz de Usuario:** Dashboards financieros, captura de pólizas manuales y visualización de Estados Financieros.

### 2. Automatización CFDI & IA (Node.js Sidecar)
Microservicios ejecutados en el entorno Linux/WSL para alimentar a Velneo:
* **Descarga Masiva:** Conector con el API de descarga del SAT.
* **Procesamiento XML/XSLT:** Transformación de CFDI a formato JSON estructurado para Velneo.
* **Clasificador IA:** Análisis de conceptos de factura para proponer el asiento contable automáticamente.

### 3. Control Presupuestal Integrado
* **Vinculación:** Relación entre el presupuesto capturado en Velneo y el gasto real detectado en los CFDI procesados.
* **Alertas:** Notificaciones visuales en Velneo cuando un gasto excede la partida presupuestal.

---


## 📂 Estructura de la Solución

```text
├── velneo_app/             # Solución de Velneo (Cajas de Datos y Aplicación)
│   ├── tablas/             # Ejercicios, Entidades, Cuentas, Pólizas, Saldos
│   └── interfaces/         # Formularios, Rejillas y Procesos V7
├── node_services/          # Microservicios en Antigravity (Node.js)
│   ├── cfdi_downloader/    # Conector con API de descarga
│   ├── ia_classifier/      # Lógica de sugerencia de cuentas
│   └── smtp_oauth/         # Envío de correos para DS-QS
└── .env                    # Keys de IA y endpoints de Velneo
```
```

---

### Instrucciones para el Asistente de Programación

Al integrar Velneo con Node.js, debemos tener cuidado con los tipos de datos:

1.  **Velneo como Maestro:** Velneo será quien tenga la "verdad" de los datos. Node.js solo actúa como un procesador que le entrega información ya digerida.
2.  **API de Comunicación:** En Node.js, crea una ruta `POST /api/v1/process-cfdi`. Este proceso recibirá el XML, hará la magia con la IA y devolverá un JSON que Velneo pueda importar fácilmente mediante sus funciones de "JSON: Importar".
3.  **Rendimiento:** Velneo es excelente manejando millones de registros de saldos. Asegúrate de que las tablas de "Saldos" en Velneo usen **actualizaciones automáticas** para que cada vez que Node.js inserte una póliza, el Balance se actualice al instante.



### 💾 Especificaciones de Datos: DS-QS (Fase 1)

Esta sección detalla la arquitectura de la base de datos en **Velneo** y el contrato de interfaz **JSON** para la automatización contable mediante IA.

---

#### 1. Esquema de Base de Datos (Estructura en Velneo V7)

El modelo en Velneo utiliza **Actualizaciones Automáticas** para garantizar que los estados financieros se calculen en tiempo real sin procesos de cierre manuales pesados.

| Tabla | Identificador | Campos Principales | Relaciones (Punteros) |
| :--- | :--- | :--- | :--- |
| **ENTIDADES** | `ID_ENT` | RFC, Razón Social, Régimen, Logo | Maestro Raíz |
| **EJERCICIOS** | `ID_EJE` | Año (2026), Estatus (A/C) | -> ENTIDADES |
| **CUENTAS** | `ID_CTA` | Código (101.01.001), Nombre, Naturaleza, Nivel | -> EJERCICIOS |
| **POLIZAS** | `ID_POL` | Fecha, Tipo (I,E,D), Concepto, UUID_SAT | -> ENTIDADES |
| **ASIENTOS** | `ID_ASI` | Debe, Haber, Referencia, Glosa | -> POLIZAS, -> CUENTAS |
| **SALDOS** | `ID_SAL` | Mes (01-12), S. Inicial, Debe_Mes, Haber_Mes | -> CUENTAS, -> EJERCICIOS |

> **Nota Técnica:** En Velneo, la tabla **SALDOS** recibe actualizaciones de la tabla **ASIENTOS**. Cada inserción en un asiento dispara un "Tubo de Actualización" que suma al Debe/Haber del saldo correspondiente de forma atómica.

---

#### 2. Estructura del JSON de Integración (Node.js ➔ Velneo)

Este JSON es el resultado del procesamiento del CFDI en **Node.js** tras pasar por el motor de **IA**. Velneo consume este objeto para dar de alta la contabilidad automáticamente.

```json
{
  "header": {
    "source": "DS-QS_AI_ENGINE",
    "version": "2.0",
    "process_id": "JOB-2026-X99",
    "ai_confidence_score": 0.96
  },
  "accounting_envelope": {
    "rfc_emisor": "ABC123456T1",
    "rfc_receptor": "XYZ987654R2",
    "fecha_documento": "2026-03-30",
    "uuid_sat": "550e8400-e29b-41d4-a716-446655440000",
    "moneda": "MXN",
    "tipo_cambio": 1.0,
    "total_factura": 1160.00
  },
  "journal_entry_proposal": {
    "tipo_poliza": "Egreso",
    "concepto_general": "Póliza automática: Adquisición de consumibles vía IA",
    "items": [
      {
        "cuenta_codigo": "601.01.001",
        "descripcion": "Gasto: Papelería y Útiles de Oficina",
        "debe": 1000.00,
        "haber": 0.00,
        "impuesto_vinculado": "IVA_16_TRAS"
      },
      {
        "cuenta_codigo": "118.01.001",
        "descripcion": "IVA Acreditable Pagado",
        "debe": 160.00,
        "haber": 0.00,
        "impuesto_vinculado": null
      },
      {
        "cuenta_codigo": "201.01.001",
        "descripcion": "Proveedores Nacionales: Papelera S.A.",
        "debe": 0.00,
        "haber": 1160.00,
        "impuesto_vinculado": null
      }
    ]
  }
}
```

---

#### 3. Flujo de Control Presupuestal

DS-QS vincula el gasto contable con el presupuesto en el momento de la inserción:
1. **Validación:** Antes de confirmar el asiento, Velneo verifica el saldo disponible en la partida presupuestal vinculada a la `cuenta_codigo`.
2. **Registro:** Si el presupuesto es suficiente, se marca el "Comprometido" como "Ejercido".
3. **Excepción:** Si excede el límite, Velneo marca la póliza con estatus "Pendiente de Autorización Presupuestal".

---

### Instrucciones para la Implementación en Antigravity

1.  **Sincronización:** Debes crear un proceso en **Velneo** que exponga el catálogo de cuentas hacia **Node.js** para que la IA siempre use códigos de cuenta existentes.
2.  **Seguridad de Datos:** El campo `uuid_sat` en Velneo debe ser un **Índice de Clave Única** para evitar que un mismo CFDI genere dos pólizas contables.
