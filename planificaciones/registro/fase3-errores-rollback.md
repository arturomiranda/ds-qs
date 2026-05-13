# FASE 3 — Mapa de Errores y Rollback
> Módulo: **Registro de Instancias Velneo + Usuario**
> Estrategia: Rollback por Capas + Dead Letter Queue (DLQ) + Checkpoints Idempotentes

---

## Tabla Maestra de Errores

| Capa | Error | Código HTTP | Efecto en DB | Efecto en Velneo | Recuperación |
|---|---|---|---|---|---|
| HTTP | DTO inválido | `422` | ❌ Ninguno | ❌ Ninguno | Cliente corrige input |
| HTTP | Email duplicado local | `400` | ❌ Ninguno | ❌ Ninguno | Cliente usa otro email |
| HTTP | Email duplicado Cloud | `400` | ❌ Ninguno | ❌ Ninguno | Cliente usa otro email |
| HTTP | OTP incorrecto | `400` | ❌ Ninguno | ❌ Ninguno | Cliente reintenta OTP |
| HTTP | JWT expirado | `400` | ❌ Ninguno | ❌ Ninguno | Nuevo registro completo |
| HTTP | Redis caído | `503`* | ❌ Ninguno | ❌ Ninguno | Fallback sesión directa |
| HTTP | Velneo Cloud caído | `503` | ❌ Ninguno | ❌ Ninguno | Retry manual posterior |
| Worker | Fallo CP1 (dat) | — | ✅ Usuario OK | ⚠️ Sin instancia dat | DLQ → retry |
| Worker | Fallo CP2 (app) | — | ✅ dat guardado | ⚠️ Sin instancia app | DLQ → retry desde CP2 |
| Worker | Fallo CP3 (group) | — | ✅ app guardado | ⚠️ Sin grupo | DLQ → retry desde CP3 |
| Worker | Fallo CP4 (user) | — | ✅ group guardado | ⚠️ Sin usuario Cloud | DLQ → retry desde CP4 |
| MySQL | Fallo en transacción | `500` | 🔄 Rollback total | ❌ Ninguno | Nuevo intento de verify |

> `*` Con fallback activo, el 503 solo ocurre si Redis cae **Y** Velneo Cloud no responde.

---

## Capa 1 — Errores HTTP Síncronos

| Error | Acción del sistema |
|---|---|
| `422` DTO inválido | Devolver campo inválido + mensaje. Sin efecto secundario. |
| `400` Email duplicado local | Log + respuesta 400. Sin efecto secundario. |
| `400` Email duplicado Cloud | Log + respuesta 400. Cerrar sesión de emergencia si fue creada. |
| `400` OTP incorrecto | Log + respuesta 400. Token sigue válido para reintentos. |
| `400` JWT expirado | Log + respuesta 400. Usuario debe reiniciar el flujo completo. |
| `503` Redis caído | Alerta métricas. Fallback a sesión directa. Degradación controlada. |
| `503` Velneo Cloud caído | Alerta crítica. Responder 503. No persistir nada. |

---

## Capa 2 — Worker: Checkpoints + Retry BullMQ

Cada checkpoint evalúa si ya fue completado antes de intentar la operación en Velneo Cloud.
Si la operación falla, BullMQ aplica backoff exponencial (2s → 4s → 8s) hasta 3 intentos.
Tras 3 fallos, el job pasa a la **Dead Letter Queue** con el estado del checkpoint alcanzado.

**Checkpoints:**
| # | Campo en `velneo` | Operación Velneo |
|---|---|---|
| CP0 | usuario en MySQL | Validación previa (sin Cloud) |
| CP1 | `id_instancia_dat_check = 1` | `PUT /instance type=data` |
| CP2 | `id_instancia_app_check = 1` | `PUT /instance type=app` |
| CP3 | `id_group_check = 1` | `PUT /group` Admin + tenant |
| CP4 | `id_user_check = 1` | `PUT /user` credenciales |

---

## Capa 3 — Dead Letter Queue (DLQ)

Jobs fallidos se conservan en `provisioning-dlq` con:
- Estado del checkpoint alcanzado (`status: failed_dat | failed_app | failed_group | failed_user`)
- `userId` y `velneoRecordId` para reanudación
- Timestamp del fallo

**Recuperación disponible:**
- `POST /api/v1/provisioning/retry/:id` (rol Admin) → re-encola desde el último checkpoint
- Dashboard de monitoreo para visualizar estado de cada aprovisionamiento
- Alerta automática al equipo ops

---

## Configuración BullMQ

```
provisioning-queue:
  attempts:         3
  backoff:          { type: exponential, delay: 2000ms }
  removeOnComplete: false
  removeOnFail:     false

mail-queue:
  attempts:         5
  backoff:          { type: fixed, delay: 3000ms }
  priority:         1

session-keeper (Cron):
  repeat:           { every: 25min }
  attempts:         2
  jobId:            'velneo-session-keeper'
```

---

> 🔴 **Riesgo crítico:** Usuario en estado **zombie** — existe en MySQL pero sin infraestructura Velneo si la DLQ no es atendida.
>
> **Mitigación:** Monitor que alerte si un `velneoRecord` lleva más de **30 minutos** sin completar `check_user = 1`.
