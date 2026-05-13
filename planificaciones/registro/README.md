# 📋 Planificación: Módulo de Registro e Instancias Velneo
> **Estado:** En planificación — Fase 6 pendiente (Prompt para Agente Ejecutor)
> **Stack:** Node/Express + Next.js + MySQL + Redis + BullMQ + Velneo Cloud API
> **Arquitectura:** Feature-Based + Clean Architecture + Workers Aislados

---

## Índice de Fases

| Fase | Documento | Estado | Descripción |
|---|---|---|---|
| 1 | [fase1-contrato-api.md](./fase1-contrato-api.md) | ✅ Completado | Endpoints, métodos, schemas Request/Response |
| 2 | [fase2-flujo-uml.md](./fase2-flujo-uml.md) | ✅ Completado | Flowchart 3 planos + Sequence Diagram multi-actor |
| 3 | [fase3-errores-rollback.md](./fase3-errores-rollback.md) | ✅ Completado | Mapa de errores HTTP + Checkpoints Worker + DLQ |
| 4 | [fase4-seguridad.md](./fase4-seguridad.md) | ✅ Completado | Rate limiting, DTOs, JWT, multi-tenant, secretos |
| 5 | [fase5-estructura-archivos.md](./fase5-estructura-archivos.md) | ✅ Completado | Feature-Based Architecture Backend + Frontend |
| 6 | `fase6-prompt-agente.md` | ⏳ Pendiente | Prompt técnico detallado para agente ejecutor |

---

## Decisiones Arquitectónicas Adoptadas

| Decisión | Elección |
|---|---|
| Cola de trabajos | **BullMQ** sobre Redis (mismo Redis para cache + queues) |
| Validación email en Velneo | **Sesión cacheada en Redis** (Cron 25min, TTL 29min) |
| Aprovisionamiento | **Asíncrono** — respuesta inmediata con `provisioningId` + polling |
| Workers | **Procesos aislados** — entry point `worker.js` independiente de `server.js` |
| Retry strategy | **Checkpoints idempotentes** — BullMQ `attempts:3, backoff:exponential` |
| Rollback | **DLQ** con recuperación vía `POST /provisioning/retry/:id` (rol Admin) |
| Seguridad JWT | HS256 + TTL diferenciado (15min registro / 8h sesión) |
| Velneo API Client | **4 servicios SRP** — Session, Instance, Group, User |

---

## Problemas del Estado Anterior Resueltos

| # | Problema | Solución |
|---|---|---|
| 🔴 1 | `Auth.service.js` mezclaba OTP + aprovisionamiento + credenciales | Separado en `Registration` + `Provisioning` |
| 🔴 2 | `Velneo.service.js`: 384 líneas, 4 responsabilidades | 4 servicios especializados (~80 líneas c/u) |
| 🔴 3 | Sin validación de entrada en controllers | DTOs Zod + middleware `validate` |
| 🟡 4 | Aprovisionamiento síncrono en request HTTP | Worker asíncrono BullMQ |
| 🟡 5 | Sin retry en checkpoints fallidos | BullMQ `attempts:3` + DLQ |
| 🔴 6 | Sin rate limiting | Middleware por endpoint |
