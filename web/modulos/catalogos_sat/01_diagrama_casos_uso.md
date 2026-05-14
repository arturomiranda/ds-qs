# 📋 Diagrama de Casos de Uso — Módulo Catálogos SAT

> **Tipo:** Use Case Diagram
> **Estándar:** UML 2.5 / C4 Context
> **Objetivo:** Modelar las fronteras del sistema y los actores involucrados en el pipeline ETL de catálogos SAT.

---

## Diagrama

```mermaid
%%{ init: { "theme": "base", "themeVariables": { "primaryColor": "#e1f5fe", "primaryBorderColor": "#01579b", "secondaryColor": "#fff3e0", "tertiaryColor": "#e8f5e9" } } }%%

graph TB
    subgraph ACTORES["👥 Actores del Sistema"]
        A1["👤 Administrador SAT\n(Responsable de Actualización)"]
        A2["🤖 Google Apps Script\n(Agente Automatizado)"]
        A3["⚙️ Motor Velneo\n(Proceso de Importación)"]
    end

    subgraph SAT["🌐 Frontera: Portal SAT (Externo)"]
        UC1["📥 UC-01\nConsultar portales oficiales SAT"]
        UC2["📄 UC-02\nDescargar archivos XLSX\n(Anexo 20 · Carta Porte · Com. Exterior)"]
        UC3["🔔 UC-03\nDetectar actualizaciones\nde catálogos"]
    end

    subgraph SHEETS["📊 Frontera: Google Workspace"]
        UC4["📤 UC-04\nCargar XLSX a Google Sheets"]
        UC5["🔤 UC-05\nNormalizar cabeceras\n(HeaderNormalizer)"]
        UC6["✅ UC-06\nEstandarizar valores\n(NFD + Regex)"]
        UC7["🔄 UC-07\nExportar hojas a JSON\n(VelneoExporter)"]
        UC8["💾 UC-08\nPersistir JSON en\nGoogle Drive /catalogos"]
    end

    subgraph VELNEO["🗄️ Frontera: Velneo / DATTA ERP"]
        UC9["📂 UC-09\nLeer JSON desde variable\nJSON_DATA o SENDA_JSON"]
        UC10["🔍 UC-10\nDetectar tabla destino\n(AnalizadorRutas)"]
        UC11["🔗 UC-11\nResolver claves foráneas\n(FKResolver)"]
        UC12["🔢 UC-12\nConvertir tipos de datos\n(ValueFormatter)"]
        UC13["💽 UC-13\nPersistir registros\npor lotes (BatchRunner)"]
        UC14["📊 UC-14\nRetornar métricas\n(inserts / updates / errores)"]
    end

    A1 --> UC1
    A1 --> UC2
    A1 --> UC3
    A1 --> UC4

    A2 --> UC5
    A2 --> UC6
    A2 --> UC7
    A2 --> UC8

    A3 --> UC9
    A3 --> UC10
    A3 --> UC11
    A3 --> UC12
    A3 --> UC13
    A3 --> UC14

    UC2 -.->|"incluye"| UC1
    UC5 -.->|"incluye"| UC6
    UC7 -.->|"incluye"| UC5
    UC8 -.->|"incluye"| UC7
    UC10 -.->|"extiende"| UC11
    UC13 -.->|"incluye"| UC12
    UC14 -.->|"incluye"| UC13

    style SAT fill:#fce4ec,stroke:#c62828,color:#333
    style SHEETS fill:#e1f5fe,stroke:#01579b,color:#333
    style VELNEO fill:#e8f5e9,stroke:#1b5e20,color:#333
    style ACTORES fill:#f3e5f5,stroke:#4a148c,color:#333
```

---

## 📌 Descripción de Casos de Uso

| ID | Caso de Uso | Actor | Descripción |
|----|-------------|-------|-------------|
| UC-01 | Consultar portales oficiales SAT | Administrador | Accede a las URLs oficiales para verificar si hay versiones nuevas de catálogos |
| UC-02 | Descargar archivos XLSX | Administrador | Descarga manualmente los archivos de los tres anexos |
| UC-03 | Detectar actualizaciones | Administrador | Compara fechas/versiones de los catálogos descargados vs. los vigentes en BD |
| UC-04 | Cargar XLSX a Google Sheets | Administrador | Importa el XLSX como hoja dentro del Spreadsheet de trabajo |
| UC-05 | Normalizar cabeceras | Apps Script | `HeaderNormalizer` aplica NFD + regex a las cabeceras de cada hoja |
| UC-06 | Estandarizar valores | Apps Script | Limpia diacríticos, convierte a mayúsculas, estandariza fechas de vigencia |
| UC-07 | Exportar hojas a JSON | Apps Script | `VelneoExporter` itera hojas y construye arrays de objetos JSON |
| UC-08 | Persistir JSON en Drive | Apps Script | `DriveService` guarda `{hoja}.json` en la carpeta `catalogos` de Drive |
| UC-09 | Leer JSON | Velneo Motor | Lee contenido desde variable `JSON_DATA` o archivo `SENDA_JSON` |
| UC-10 | Detectar tabla destino | Velneo Motor | `AnalizadorRutas` identifica la tabla Velneo usando la `pistaJSON` |
| UC-11 | Resolver claves foráneas | Velneo Motor | `FKResolver` resuelve IDs de MUN/LOC para la tabla A20_CP |
| UC-12 | Convertir tipos | Velneo Motor | `ValueFormatter` transforma string/fecha/boolean a tipos Velneo |
| UC-13 | Persistir por lotes | Velneo Motor | `BatchRunner` procesa lotes de 1 000 registros con transacción |
| UC-14 | Retornar métricas | Velneo Motor | Devuelve resumen `OK\|Éxito: N registros` en variable `RETORNO` |

---

## 🔗 Relaciones Notables

- **Incluye (`<<include>>`):** UC-07 siempre ejecuta UC-05 (la exportación depende de la normalización previa).
- **Extiende (`<<extend>>`):** UC-11 sólo se activa cuando la tabla destino es `A20_CP` (códigos postales con FK a municipio y localidad).
- **Frontera crítica:** El límite entre Google Workspace y Velneo se materializa en el archivo `.json` en Google Drive — es el único contrato de interfaz entre ambos sistemas.
