# 📂 Arquitectura de Módulos y Estrategia de API

Este documento detalla la estructura modular de **Datta-Erp** y cómo se integran los servicios de Backend (Node/Express) con el Frontend (Next.js) y las bases de datos (Velneo/MySQL).

## 🧩 Desglose de Módulos Actuales y Propuestos

### 1. Módulo de Autenticación (`Auth`)
- **Backend**: `/src/modules/Auth`
- **Frontend**: Integrado en `providers/` y `middleware.ts`.
- **Propósito**: Gestión de sesiones, JWT, permisos multi-tenant y seguridad.
- **Base de Datos**: MySQL para metadatos de usuario; Velneo para perfiles de empleado.

### 2. Módulo de Catálogos (`Catalogos`)
- **Backend**: Pendiente de modularizar (actualmente en rutas genéricas).
- **Frontend**: `/modules/Catalogos`
- **Propósito**: Administración de entidades básicas (Clientes, Proveedores, Artículos, Almacenes).
- **Base de Datos**: Velneo Cloud (Core).

### 3. Módulo de Sincronización Velneo (`Velneo`)
- **Backend**: `/src/modules/Velneo`
- **Propósito**: Capa de abstracción para la comunicación con Velneo V7 (VRL/RPC/REST).
- **Lógica**: Centraliza las llamadas a procesos de Velneo para evitar duplicidad de código.

---

## 📡 Estrategia de API (Contratos)

Para garantizar la consistencia, todos los nuevos módulos deben seguir este estándar de respuesta:

```json
{
  "success": true,
  "data": { ... },
  "message": "Operación exitosa",
  "meta": {
    "count": 100,
    "page": 1
  }
}
```

### Endpoints Base Propuestos:

| Método | Endpoint | Módulo | Descripción |
| :--- | :--- | :--- | :--- |
| `POST` | `/api/auth/login` | Auth | Inicio de sesión y generación de JWT. |
| `GET` | `/api/catalogos/:tipo` | Catalogos | Obtención de listas de catálogos desde Velneo. |
| `POST` | `/api/velneo/execute` | Velneo | Ejecución de procesos remotos en Velneo Cloud. |

---

## 🛠️ Estructura de Directorios (Feature-Based)

### Backend
```text
src/
└── modules/
    ├── Auth/
    │   ├── auth.controller.js
    │   ├── auth.service.js
    │   └── auth.routes.js
    ├── Catalogos/
    └── Velneo/
```

### Frontend
```text
modules/
└── Catalogos/
    ├── components/
    ├── hooks/
    ├── types/
    └── CatalogosPage.tsx
```

## 🔴 Análisis de Riesgos y Mitigación
- **Riesgo**: Latencia en llamadas a Velneo Cloud.
- **Mitigación**: Implementar caching en el Backend con Redis o en memoria para catálogos estáticos, y usar `TanStack Query` en el Frontend para revalidación en segundo plano.
