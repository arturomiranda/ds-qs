# ⏱️ Diagrama de Secuencia — Pipeline ETL SAT → Velneo

> **Tipo:** Sequence Diagram
> **Estándar:** UML 2.5
> **Objetivo:** Visualizar el flujo cronológico completo de datos desde los portales oficiales del SAT hasta la persistencia final en la base de datos DATTA ERP.

---

## Fase 1 — Sourcing y Normalización (Google Workspace)

```mermaid
sequenceDiagram
    autonumber
    actor Admin as 👤 Administrador
    participant SAT as 🌐 Portal SAT
    participant Sheets as 📊 Google Sheets
    participant HN as 🔤 HeaderNormalizer
    participant SHM as ⚙️ SpreadsheetHeaderManager
    participant DT as 🔧 DataTransformer

    Note over Admin,SAT: ── FASE 1: SOURCING ──────────────────────────────

    Admin ->> SAT: GET /tramitesyservicios/Paginas/anexo_20.htm
    SAT -->> Admin: Página con vínculos a XLSX
    Admin ->> SAT: Descarga XLSX (Anexo 20 / Carta Porte / Com. Exterior)
    SAT -->> Admin: archivo.xlsx

    Admin ->> Sheets: Importa XLSX como hoja activa
    Sheets -->> Admin: Hoja cargada con datos crudos

    Note over Sheets,DT: ── FASE 2: NORMALIZACIÓN DE CABECERAS ─────────────

    Admin ->> SHM: runHeaderStandardization()
    activate SHM
    SHM ->> Sheets: getActiveSpreadsheet().getSheets()
    Sheets -->> SHM: [Sheet, Sheet, ...]

    loop Por cada hoja
        SHM ->> Sheets: sheet.getRange(1,1,1,lastCol)
        Sheets -->> SHM: rawValues[]
        SHM ->> HN: normalize(cellValue)
        activate HN
        HN ->> HN: toUpperCase() + normalize("NFD")
        HN ->> HN: replace(/[\u0300-\u036f]/g, "")
        HN ->> HN: Evaluar reglas de negocio (Regex)
        HN -->> SHM: valorNormalizado
        deactivate HN
        SHM ->> Sheets: headerRange.setValues([processedValues])
    end

    SHM -->> Admin: "Estandarización completada"
    deactivate SHM
```

---

## Fase 2 — Serialización y Almacenamiento en Drive

```mermaid
sequenceDiagram
    autonumber
    actor Admin as 👤 Administrador
    participant Sheets as 📊 Google Sheets
    participant VE as 📦 VelneoExporter
    participant DT as 🔧 DataTransformer
    participant DS as 💾 DriveService
    participant Drive as ☁️ Google Drive

    Note over Admin,Drive: ── FASE 3: EXPORTACIÓN JSON ────────────────────────

    Admin ->> VE: runExportProcess()
    activate VE

    VE ->> DS: new DriveService("catalogos")
    activate DS
    DS ->> Drive: getFoldersByName("catalogos")
    alt Carpeta existe
        Drive -->> DS: Folder existente
    else No existe
        DS ->> Drive: createFolder("catalogos")
        Drive -->> DS: Nueva Folder
    end
    DS -->> VE: driveService listo
    deactivate DS

    VE ->> Sheets: getActiveSpreadsheet().getSheets()
    Sheets -->> VE: [Sheet, Sheet, ...]

    loop Por cada hoja
        VE ->> Sheets: sheet.getDataRange()
        Sheets -->> VE: rawValues[][], displayValues[][]

        VE ->> DT: cleanText(headerCell)
        DT -->> VE: cabecera limpia

        loop Por cada fila de datos
            VE ->> DT: formatValue(raw, display)
            activate DT
            alt raw instanceof Date
                DT -->> VE: "YYYY-MM-DD"
            else typeof raw === boolean
                DT -->> VE: "1" | "0"
            else
                DT -->> VE: cleanText(display)
            end
            deactivate DT
        end

        VE ->> DS: saveJson(sheetName, records[])
        activate DS
        DS ->> Drive: folder.createFile("{hoja}.json", JSON.stringify(content), "application/json")
        Drive -->> DS: File URL
        DS -->> VE: fileUrl
        deactivate DS
        VE ->> VE: results.push({ name, url })
    end

    VE -->> Admin: renderModal(links, folderUrl)
    deactivate VE

    Note over Admin,Drive: ── Resultado: N archivos {catalogo}.json en Drive ──
```

---

## Fase 3 — Importación en Velneo

```mermaid
sequenceDiagram
    autonumber
    participant Velneo as ⚙️ Proceso Velneo
    participant JL as 📂 JSONLoader
    participant AR as 🔍 AnalizadorRutas
    participant IS as 🎯 ImportService
    participant IR as 🗂️ ImporterRegistry
    participant IMP as 🔄 Importer
    participant DM as 🗺️ DataMapper
    participant VF as 🔢 ValueFormatter
    participant FKR as 🔗 FKResolver
    participant VR as 🗄️ VelneoRepository
    participant BR as 📦 BatchRunner
    participant BD as 💽 catalogos_sat_dat

    Note over Velneo,BD: ── FASE 4: IMPORTACIÓN EN VELNEO ──────────────────

    Velneo ->> JL: parseFromVariable(JSON_DATA)
    activate JL
    JL ->> JL: sanitize() — elimina BOM y espacios
    JL ->> JL: JSON.parse(sanitized)
    JL -->> Velneo: data[] (array de registros)
    deactivate JL

    Velneo ->> AR: analizar(data[0])
    activate AR
    AR ->> AR: Buscar pistaJSON en DiccionarioTablas
    AR -->> Velneo: MappingRule { pistaJSON, tablaVelneo, mapeo[] }
    deactivate AR

    Velneo ->> BR: processAll(rule, data, 1000)
    activate BR

    loop Por cada lote de 1000 registros
        BR ->> BR: theRoot.beginTrans("Lote N/M")

        loop Por cada registro del lote
            BR ->> IS: importRecord(rule, record)
            activate IS
            IS ->> IR: getImporter(rule.tablaVelneo)

            alt tablaVelneo === "catalogos_sat_dat/A20_CP"
                IR -->> IS: CPImporter
                IS ->> IMP: execute(rule, record) [CPImporter]
                activate IMP
                IMP ->> DM: extract(record, "MUNICIPIO")
                DM -->> IMP: munClv
                IMP ->> DM: extract(record, "LOCALIDAD")
                DM -->> IMP: locClv
                IMP ->> FKR: resolve("A20_MUN", "catalogos_sat_dat", "EST", [estCode], {CLV: munClv})
                activate FKR
                FKR ->> BD: VRegisterList.load("EST", [estCode])
                BD -->> FKR: registros coincidentes
                FKR ->> FKR: Filtrar por CLV === munClv
                FKR -->> IMP: munId (número)
                deactivate FKR
                IMP ->> FKR: resolve("A20_LOC", ..., {CLV: locClv})
                FKR -->> IMP: locId (número)
                deactivate IMP
            else Default
                IR -->> IS: Importers.Default
                IS ->> IMP: execute(rule, record) [Default]
                activate IMP
                IMP ->> IMP: _findExisting() — buscar por ID/CLV
            end

            loop Por cada campo del mapeo
                IMP ->> DM: extract(record, m.json)
                DM -->> IMP: rawValue
                IMP ->> VF: format(rawValue, m.tipo)
                VF -->> IMP: valorFormateado
                IMP ->> VR: reg.setField(m.velneo, valorFormateado)
            end

            IMP ->> VR: persist(reg, isUpdate)
            activate VR
            alt isUpdate
                VR ->> BD: reg.modifyRegister()
                BD -->> VR: OK
                VR -->> IMP: "update"
            else insert
                VR ->> BD: reg.addRegister()
                BD -->> VR: OK
                VR -->> IMP: "insert"
            end
            deactivate VR
            deactivate IMP
            IS -->> BR: "insert" | "update"
            deactivate IS
        end

        alt inserts > 0 || updates > 0
            BR ->> BD: theRoot.commitTrans()
        else sólo errores
            BR ->> BD: theRoot.rollbackTrans()
        end
    end

    BR -->> Velneo: { inserts, updates, fails }
    deactivate BR

    Velneo ->> Velneo: setVar("RETORNO", "OK|Éxito: N registros")
    Note over Velneo,BD: ── Fin del proceso ─────────────────────────────────
```

---

## 📌 Notas de Arquitectura

| Aspecto | Observación |
|---------|------------|
| **Contrato de interfaz** | El archivo `.json` en Google Drive es el único punto de acoplamiento entre Workspace y Velneo. Un cambio en el esquema de cabeceras del XLSX rompe la cadena completa. |
| **Fallback de lectura** | El motor intenta primero `JSON_DATA` (variable en memoria); si está vacía, recurre a `SENDA_JSON` (ruta de archivo físico). Esto permite ejecución tanto vía API como manual. |
| **Transaccionalidad parcial** | Por diseño, se hace `commitTrans()` aunque existan fallos parciales dentro del lote. Los errores se acumulan en `metrics.fails` pero no bloquean los registros correctos. |
| **Detección de tabla** | `AnalizadorRutas` usa la primera clave JSON del primer registro como "pista". Si un JSON vacío o malformado llega al motor, lanza `Error` inmediato. |
