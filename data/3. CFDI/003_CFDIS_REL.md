# Tabla: 003: CFDIS Relacionados

Esta tabla permite vincular un CFDI con otros comprobantes previos (ej. notas de crédito vinculadas a facturas, sustitución de CFDI previos, etc.).

## 📄 Información General
- **ID de Tabla:** `003_CFDIS_REL`
- **Tipo:** Maestro
- **Reside en:** Disco
- **Longitud del registro:** 49
- **Número de campos:** 4
- **Número de índices:** 2
- **Descripción:** Vinculación entre comprobantes fiscales relacionados.

## 🛠️ Estructura de Campos

| Identificador | Nombre | Tipo | Longitud | Tipo de enlace |
| :--- | :--- | :--- | :--- | :--- |
| `ID` | Código | Numérico | 3 | |
| `CFDI_PAD` | CFDI Padre | Numérico | 3 | Maestro: 003_CFDI@datta_erp_dat |
| `UUID_REL` | UUID Relacionado | Alfa 256 | 40 | |
| `TIP` | Tipo | Alfa 256 | 2 | |

## 🔍 Índices

| Identificador | Nombre | Tipo de indice |
| :--- | :--- | :--- |
| `ID` | Código | Clave única |
| `CFDI_PAD` | CFDI Padre | Acepta repetidas |

## 📝 Notas
- El campo `TIP` almacena la clave del SAT para el tipo de relación (ej. 01 para Nota de crédito).
- Permite cumplir con los requisitos de trazabilidad fiscal del SAT para comprobantes relacionados.
