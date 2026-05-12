# 📐 Especificación Técnica — Pipeline ETL Catálogos SAT

> **Tipo:** Documento de Especificación Técnica
> **Versión:** 1.0.0
> **Módulo:** Catálogos SAT (Anexo 20 · Carta Porte · Comercio Exterior)
> **Sistema destino:** DATTA ERP — Velneo
> **Fecha:** 2026-05-12

---

## 1. Descripción General

El Módulo de Catálogos SAT implementa un pipeline **ETL híbrido manual-automatizado** que sincroniza los catálogos fiscales oficiales publicados por el SAT con la base de datos `catalogos_sat_dat` del sistema DATTA ERP en Velneo.

El flujo se divide en **cuatro fases** ejecutadas en plataformas distintas:

| Fase | Nombre | Plataforma | Automatización |
|------|--------|-----------|---------------|
| 1 | Sourcing | Portal SAT | ❌ Manual |
| 2 | Normalización de cabeceras | Google Apps Script | ✅ Automático |
| 3 | Serialización JSON | Google Apps Script | ✅ Automático |
| 4 | Importación a BD | Velneo JavaScript | ✅ Automático |

---

## 2. Fuentes Oficiales SAT

### 2.1 URLs de Monitoreo

| Normativa | URL | Formato |
|-----------|-----|---------|
| **Anexo 20** | http://omawww.sat.gob.mx/tramitesyservicios/Paginas/anexo_20.htm | XLSX |
| **Carta Porte** | http://omawww.sat.gob.mx/tramitesyservicios/Paginas/complemento_carta_porte.htm | XLSX |
| **Comercio Exterior** | http://omawww.sat.gob.mx/informacion_fiscal/factura_electronica/Paginas/Catalogos_comercio_exterior.aspx | XLSX |

### 2.2 Protocolo de Monitoreo

1. Consultar las URLs periódicamente (se recomienda frecuencia mensual mínima).
2. Comparar la fecha de publicación o versión del XLSX descargado contra la última importación registrada.
3. Si hay cambios, iniciar el proceso de actualización desde la Fase 1.
4. Registrar la versión importada en el log de actualizaciones del proyecto.

> ⚠️ **Advertencia:** El SAT no publica un feed de actualizaciones; la detección es completamente manual. Un catálogo desactualizado puede provocar rechazos de CFDI por claves inválidas.

---

## 3. Fase 1 — Sourcing

### 3.1 Procedimiento

```
1. Acceder a la URL oficial del catálogo correspondiente.
2. Localizar el vínculo de descarga del archivo XLSX (generalmente "Catálogos del SAT").
3. Descargar el archivo XLSX a equipo local.
4. Abrir Google Sheets y cargar el XLSX:
   Archivo → Importar → Subir → Reemplazar hoja actual
5. Verificar que todas las hojas del XLSX se cargaron correctamente.
```

### 3.2 Estructura esperada del XLSX

Cada hoja del XLSX representa un catálogo independiente. La primera fila contiene las cabeceras de columnas.

**Ejemplo — Hoja `c_FormaPago` (Anexo 20):**

| C_FORMAPAGO | Descripcion | Bancarizado | Numero de Operacion | Fecha Inicio de Vigencia | Fecha Fin de Vigencia |
|------------|-------------|-------------|--------------------|--------------------------|-----------------------|
| 01 | Efectivo | No | No Aplica | 2017-01-01 | |
| 02 | Cheque nominativo | Sí | Sí | 2017-01-01 | |

---

## 4. Fase 2 — Normalización de Cabeceras

### 4.1 Componentes

| Componente | Archivo | Responsabilidad (SRP) |
|-----------|---------|----------------------|
| `HeaderNormalizer` | `normalizacion.gs` | Procesamiento de texto de cabeceras |
| `SpreadsheetHeaderManager` | `normalizacion.gs` | Orquestación de todas las hojas |

### 4.2 Algoritmo de Normalización

```
Para cada celda de cabecera (fila 1):
  1. Si el valor es null, vacío o no-string → retornar sin cambios
  2. Aplicar .toUpperCase()
  3. Aplicar .normalize("NFD")  ← Descompone caracteres Unicode
  4. Aplicar .replace(/[\u0300-\u036f]/g, "")  ← Elimina marcas diacríticas
  5. Aplicar .trim()
  6. Evaluar reglas de negocio (orden de prioridad):
     - /FECHA.*INICIO.*VIGEN.*/i → "FECHA INICIO DE VIGENCIA"
     - /FECHA.*FIN.*VIGEN.*/i   → "FECHA FIN DE VIGENCIA"
  7. Si ninguna regla aplica → retornar valor ya normalizado
```

### 4.3 Transformaciones de Ejemplo

| Valor crudo (XLSX) | Valor normalizado (resultado) |
|--------------------|-------------------------------|
| `Descripción` | `DESCRIPCION` |
| `Fecha inicio de vigencia` | `FECHA INICIO DE VIGENCIA` |
| `Fecha Fin de Vigencia` | `FECHA FIN DE VIGENCIA` |
| `Número de Operación` | `NUMERO DE OPERACION` |
| `Bancarizado` | `BANCARIZADO` |

### 4.4 Punto de entrada

```javascript
// Ejecutar desde el menú de Apps Script o manualmente:
runHeaderStandardization();
```

---

## 5. Fase 3 — Serialización JSON

### 5.1 Componentes

| Componente | Archivo | Responsabilidad (SRP) |
|-----------|---------|----------------------|
| `DataTransformer` | `json.gs` | Transformación de tipos de datos |
| `DriveService` | `json.gs` | Persistencia de archivos en Google Drive |
| `VelneoExporter` | `json.gs` | Orquestación de exportación por hojas |

### 5.2 Reglas de Transformación de Tipos

| Tipo de dato fuente | Lógica aplicada | Tipo destino JSON |
|--------------------|-----------------|-------------------|
| `Date` (objeto JS) | `new Date(raw - tzOffset).toISOString().substring(0,10)` | `string` — formato `YYYY-MM-DD` |
| `boolean` (true) | `"1"` | `string` |
| `boolean` (false) | `"0"` | `string` |
| `string` / número / otro | `normalize("NFC").trim().replace(/\r?\n\|\r/g, " ")` | `string` limpio |
| `""` / `null` / `undefined` | `""` (cadena vacía) | Se omite el campo en el record |

### 5.3 Estructura del JSON generado

```json
[
  {
    "C_FORMAPAGO": "01",
    "DESCRIPCION": "Efectivo",
    "BANCARIZADO": "No",
    "NUMERO DE OPERACION": "No Aplica",
    "FECHA INICIO DE VIGENCIA": "2017-01-01",
    "FECHA FIN DE VIGENCIA": ""
  },
  {
    "C_FORMAPAGO": "02",
    "DESCRIPCION": "Cheque nominativo",
    "BANCARIZADO": "Si",
    "NUMERO DE OPERACION": "Si",
    "FECHA INICIO DE VIGENCIA": "2017-01-01",
    "FECHA FIN DE VIGENCIA": ""
  }
]
```

### 5.4 Almacenamiento en Google Drive

- **Carpeta destino:** `catalogos` (se crea automáticamente si no existe)
- **Nombre de archivo:** `{nombre_de_la_hoja}.json`
- **Codificación:** `application/json; charset=utf-8`
- **Formato:** JSON indentado con 2 espacios (`JSON.stringify(content, null, 2)`)

### 5.5 Punto de entrada

```
Menú de Google Sheets → 📦 Exportar JSON → Exportar para Velneo (Pro)
```

---

## 6. Fase 4 — Importación en Velneo

### 6.1 Arquitectura del Motor

El motor de importación (`motor_importacion.js`) aplica los patrones de diseño **Strategy**, **Registry**, **Repository** y **Service Layer (Facade)** bajo los principios SOLID.

```
motor_importacion.js
├─ import_globals.js          → MappingRule, DiccionarioTablas (base)
├─ analizador_anexo20.js      → Reglas A20_* (25 tablas)
├─ analizador_CartaPorte.js   → Reglas CP_*  (27 tablas)
├─ fk_resolver.js             → FKResolver (resolución de claves foráneas)
└─ [Namespace principal]
   ├─ JSONLoader               → Parseo y sanitización del JSON
   ├─ AnalizadorRutas         → Detección de tabla destino
   ├─ ValueFormatter          → Conversión de tipos (Strategy)
   ├─ DataMapper              → Extracción de valores por campo/clave
   ├─ VelneoRepository        → Abstracción de VRegister (Repository)
   ├─ ImporterRegistry        → Selector de estrategia (Registry/Factory)
   ├─ Importers.Default       → Importador estándar (Strategy)
   ├─ Importers.CPImporter    → Importador con FK (Strategy especializada)
   ├─ ImportService           → Coordinación por registro (Facade)
   └─ BatchRunner             → Procesamiento transaccional por lotes
```

### 6.2 Variables de Comunicación con Velneo

| Variable | Dirección | Tipo | Descripción |
|----------|-----------|------|-------------|
| `JSON_DATA` | Entrada | String | Contenido JSON completo (prioridad 1) |
| `SENDA_JSON` | Entrada | String | Ruta al archivo `.json` físico (prioridad 2, fallback) |
| `RETORNO` | Salida | String | Resultado del proceso (`OK\|...` o `ERROR\|...`) |

### 6.3 Estrategia de Detección de Tabla (`AnalizadorRutas`)

El motor analiza la **primera clave del primer registro JSON** y la compara contra la propiedad `pistaJSON` de cada `MappingRule` registrado en `DiccionarioTablas`:

```javascript
// Ejemplo: si data[0] tiene la clave "C_FORMAPAGO"
// AnalizadorRutas retorna la regla:
{
  pistaJSON: "C_FORMAPAGO",
  tablaVelneo: "catalogos_sat_dat/A20_FOR_PAG",
  mapeo: [
    { velneo: "ID",         json: "C_FORMAPAGO",          tipo: "string" },
    { velneo: "NAME",       json: "DESCRIPCION",           tipo: "string" },
    { velneo: "BANC",       json: "BANCARIZADO",           tipo: "string" },
    { velneo: "NUM_OPE",    json: "NUMERO DE OPERACION",   tipo: "string" },
    { velneo: "FEC_INI_VIG",json: "FECHA INICIO DE VIGENCIA", tipo: "fecha" },
    { velneo: "FEC_FIN_VIG",json: "FECHA FIN DE VIGENCIA",    tipo: "fecha" }
  ]
}
```

### 6.4 Estrategias de Importación

#### 6.4.1 `Importers.Default` — Mapeo Estándar

Aplica para todas las tablas **excepto** `A20_CP`.

```
Algoritmo:
1. Buscar registro existente:
   a. Si mapeo tiene campo "ID"  → buscar por índice "ID"  [valor del JSON]
   b. Si mapeo tiene campo "CLV" → buscar por índice "CLV" [valor del JSON]
   c. Si tiene "CLV" + "EST"     → buscar por índice "EST_MUN" [valEST, valCLV]
2. Si existe → isUpdate = true, reutilizar VRegister
3. Si no existe → isUpdate = false, crear nuevo VRegister
4. Para cada campo en rule.mapeo:
   - Si isUpdate y m.velneo === "ID" → omitir (evitar sobreescritura de PK)
   - rawValue = DataMapper.extract(record, m.json)
   - formattedValue = ValueFormatter.format(rawValue, m.tipo)
   - reg.setField(m.velneo, formattedValue)
5. VelneoRepository.persist(reg, isUpdate) → "insert" | "update"
```

#### 6.4.2 `Importers.CPImporter` — Códigos Postales con FK

Aplica **únicamente** para la tabla `catalogos_sat_dat/A20_CP`.

```
Algoritmo adicional (resolución de FK antes de persistir):
1. Extraer: cpId, estCode, munClv, locClv del registro JSON
2. Resolver FK de Municipio:
   - FKResolver.resolve("A20_MUN", "catalogos_sat_dat", "EST", [estCode], { CLV: munClv })
   - Si munClv existe pero munId es null → throw Error (integridad referencial)
3. Resolver FK de Localidad:
   - FKResolver.resolve("A20_LOC", "catalogos_sat_dat", "EST", [estCode], { CLV: locClv })
   - Si locClv existe pero locId es null → throw Error
4. reg.setField("MUN", munId)  ← ID numérico interno de Velneo
5. reg.setField("LOC", locId)  ← ID numérico interno de Velneo
6. Aplicar resto del mapeo (excepto ID, EST, MUN, LOC ya seteados)
7. Persistir
```

### 6.5 Conversión de Tipos (`ValueFormatter`)

| Tipo declarado | Estrategia aplicada | Ejemplo entrada | Ejemplo salida |
|---------------|--------------------|--------------|--------------| 
| `string` (default) | `String(v)` o `""` si null | `"01"` | `"01"` |
| `numero` | `Number(v)` o `0` si null | `"4"` | `4` |
| `fecha` | `String(v).substring(0,10)` | `"2017-01-01T00:00:00Z"` | `"2017-01-01"` |
| `booleano` | Evalúa: `si/sí/yes/1/true → 1`, resto `→ 0` | `"Sí"` | `1` |

### 6.6 Procesamiento por Lotes (`BatchRunner`)

| Parámetro | Valor | Descripción |
|-----------|-------|-------------|
| `batchSize` | `1000` | Registros por lote transaccional |
| Transacción | `theRoot.beginTrans()` | Una transacción por lote |
| Commit | Si `inserts > 0 \|\| updates > 0` | Parcial: registros correctos se guardan |
| Rollback | Si todos fallaron | Solo si el lote completo es fallido |

### 6.7 Formato de Retorno

```
Éxito total:
  RETORNO = "OK|Éxito: 150 registros (Nuevos: 120, Actualizados: 30)"

Éxito con errores parciales:
  RETORNO = "OK|Éxito: 148 registros (Nuevos: 118, Actualizados: 30) | Errores: 2"

Error crítico:
  RETORNO = "ERROR|No se recibieron datos JSON (Las variables JSON_DATA y SENDA_JSON están vacías)."
  RETORNO = "ERROR|No se encontró ninguna tabla relacionada para este archivo JSON en el diccionario global."
```

---

## 7. Contrato de Interfaz JSON (Google Drive ↔ Velneo)

El único punto de acoplamiento entre la capa Google Workspace y Velneo es el archivo `.json` almacenado en Google Drive. El contrato debe cumplir:

| Regla | Descripción |
|-------|------------|
| **Formato** | Array JSON `[{}, {}, ...]` — nunca objeto raíz `{}` |
| **Codificación** | UTF-8 sin BOM (el motor elimina BOM automáticamente) |
| **Primera clave** | Debe coincidir exactamente con `pistaJSON` de un `MappingRule` registrado |
| **Nombres de campos** | Deben corresponder a las cabeceras normalizadas (post-HeaderNormalizer) |
| **Valores nulos** | Campos con valor `""` o `null` son omitidos del `record` en AppScript |
| **Fechas** | Formato `YYYY-MM-DD` (string, no objeto Date) |
| **Booleanos** | `"0"` o `"1"` como string (mapeados desde `true/false` en Sheets) |

---

## 8. Extensibilidad del Sistema

### 8.1 Agregar un Nuevo Catálogo (e.g., Comercio Exterior)

1. Crear archivo `analizador_ComercioExterior.js` siguiendo el patrón de `analizador_anexo20.js`.
2. Definir `MappingRule` para cada tabla con su `pistaJSON` y mapeo de campos.
3. Agregar `#include` del nuevo archivo en `motor_importacion.js`.
4. Si alguna tabla requiere FK → registrar en `ImporterRegistry.map`.

### 8.2 Agregar un Nuevo Tipo de Conversión

```javascript
// Extender ValueFormatter sin modificar el núcleo (OCP):
ValueFormatter.addStrategy("porcentaje", function(v) {
  return v === null ? 0 : parseFloat(v) / 100;
});
```

### 8.3 Agregar una Regla de Normalización de Cabecera

```javascript
// En normalizacion.gs → constructor de HeaderNormalizer:
this.rules.push({
  pattern: /DESCRIPCION.*CORTA.*/i,
  replacement: "DESCRIPCION CORTA"
});
```

---

## 9. Dependencias y Compatibilidad

| Componente | Tecnología | Versión / Notas |
|-----------|-----------|----------------|
| Normalización | Google Apps Script (V8 Runtime) | ES6+ (clases, arrow functions) |
| Serialización | Google Apps Script (V8 Runtime) | ES6+ |
| Velneo Motor | Velneo JavaScript Engine | ES5 compatible (`var`, sin arrow fn) |
| API Velneo | `VRegister`, `VRegisterList`, `VFile`, `VTextFile` | Clases nativas de Velneo |
| BD destino | `catalogos_sat_dat` | Senda completa: `catalogos_sat_dat/A20_*` o `CP_*` |

---

## 10. Registro de Actualizaciones Recomendado

Se recomienda mantener el siguiente registro en el repositorio cada vez que se actualice un catálogo:

```markdown
| Fecha      | Catálogo         | Versión SAT | Tablas afectadas | Responsable | Resultado |
|-----------|-----------------|------------|-----------------|-------------|-----------|
| 2026-05-12 | Anexo 20        | 4.0         | A20_FOR_PAG     | W. Miranda  | OK — 32 registros |
| 2026-05-12 | Carta Porte     | 3.1         | CP_EST, CP_CLV_TRAN | W. Miranda | OK — 118 reg |
```

---

## 11. Notas de Arquitectura

> **[IA] Riesgo: Cambio de esquema SAT.** Si el SAT renombra o agrega columnas al XLSX, el `HeaderNormalizer` sólo aplica 2 reglas explícitas. Cualquier nueva columna pasará con su nombre crudo normalizado. Si ese nombre no coincide con el campo `json` en `MappingRule`, el valor será silenciosamente ignorado durante la importación.

> **[IA] Riesgo: Rendimiento en FKResolver.** La función `resolve()` carga el índice EST completo y filtra en memoria. Para catálogos de Códigos Postales con decenas de miles de registros, esto genera N llamadas a la BD (una por municipio/localidad único). **Mitigación recomendada:** Implementar un `Map` de caché en memoria dentro del contexto del `BatchRunner`.

> **[IA] Fortaleza: Transaccionalidad parcial.** El diseño de `BatchRunner` permite que un lote con fallos parciales preserve los registros correctos (`commitTrans` si `inserts > 0 || updates > 0`). Esto es deseable para catálogos grandes donde un registro corrupto no debe bloquear los demás.

> **[IA] Escalabilidad: Patrón Strategy.** La arquitectura `ImporterRegistry` + `Importers.*` permite incorporar los catálogos de Comercio Exterior sin modificar el motor central. Es el camino correcto para extender el sistema.
