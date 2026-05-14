# 🔄 Flujos de Procesos y Lógica de Integración

Este documento visualiza los procesos críticos de **Datta-Erp**, desde el registro de un nuevo cliente hasta el consumo de datos del ERP.

## 1. Flujo de Registro y Activación (Onboarding)

Este diagrama detalla cómo un nuevo usuario obtiene acceso a su instancia de Velneo Cloud.

```mermaid
sequenceDiagram
    participant U as Usuario (Next.js)
    participant B as Backend (Express)
    participant M as MySQL (Orquestador)
    participant V as Velneo API

    U->>B: Registro (Datos + RFC)
    B->>M: Insertar en 'usuarios' (verificado=0)
    B->>B: Generar OTP
    B->>U: Solicitar Verificación OTP
    U->>B: Envía OTP
    B->>M: Validar OTP y Activar Usuario
    
    Note over B,V: Proceso de Aprovisionamiento
    B->>M: Buscar Carpeta Disponible
    M-->>B: Retorna 'id_carpeta'
    B->>V: Crear Instancias (DAT/APP)
    V-->>B: IDs de Instancia + URL API
    B->>M: Registrar en tabla 'velneo'
    B-->>U: ¡Listo! Redirigir a Dashboard
```

## 2. Flujo de Consulta de Datos ERP (Dashboard)

Cómo el Frontend consume información del core de Velneo a través de la capa intermedia de Node.js.

```mermaid
flowchart TD
    A[Usuario entra al Dashboard] --> B{¿Hay Sesión?}
    B -- No --> C[Redirigir Login]
    B -- Sí --> D[Frontend: TanStack Query solicita /api/dashboard]
    
    D --> E[Backend: Middleware verifica JWT]
    E --> F[Backend: Consulta MySQL tabla 'velneo' por id_usuario]
    F --> G[Obtiene URL_API de la instancia del cliente]
    
    G --> H[Backend: Request HTTP a Velneo V7 REST]
    H --> I[Velneo: Ejecuta Proceso V7 / Búsqueda]
    I --> J[Velneo: Retorna JSON]
    
    J --> K[Backend: Sanitiza y Formatea Datos]
    K --> L[Frontend: TanStack Query almacena en Cache]
    L --> M[Renderizar Gráficas y Tablas]

    style A fill:#f9f,stroke:#333,stroke-width:2px
    style I fill:#00ff00,stroke:#333,stroke-width:2px
    style M fill:#00ffff,stroke:#333,stroke-width:2px
```

## 3. Manejo de Errores y Rollback (Resiliencia)

Estrategia para evitar estados inconsistentes durante el aprovisionamiento.

| Escenario de Falla | Acción de Mitigación | Estado Final |
| :--- | :--- | :--- |
| Error al crear instancia en Velneo | Eliminar registro parcial en MySQL (`usuarios`) y notificar error. | Limpio |
| Fallo en envío de correo OTP | Reintentar envío (máx 3) o permitir reenvío manual desde UI. | Pendiente de Verificación |
| Velneo API Offline | Backend retorna `503 Service Unavailable` con mensaje amigable. | Reintentable |

---

## 🎨 Estilos de Interfaz (UX/UI Strategy)
- **Dashboard**: Uso de `Lucide-React` para iconografía minimalista.
- **Feedback**: `React-Hot-Toast` para confirmaciones de guardado y errores de API.
- **Persistencia**: `js-cookie` para tokens de corta duración y seguridad `HttpOnly` en producción.
