# Tabla: 003: Complementos

Esta tabla almacena los complementos adicionales de un CFDI (ej. Complemento de Pago, Nómina, Comercio Exterior, etc.) en formato estructurado.

## 📄 Información General
- **ID de Tabla:** `003_CPLM`
- **Tipo:** Maestro
- **Reside en:** Disco
- **Longitud del registro:** 65
- **Número de campos:** 4
- **Número de índices:** 2
- **Descripción:** Registro de complementos fiscales vinculados a un CFDI.

## 🛠️ Estructura de Campos

| Identificador | Nombre | Tipo | Longitud | Tipo de enlace |
| :--- | :--- | :--- | :--- | :--- |
| `ID` | Código | Numérico | 3 | |
| `CFDI` | CFDI | Numérico | 3 | Maestro: 003_CFDI@datta_erp_dat |
| `TIP` | Tipo | Alfa 256 | 50 | |
| `JSN_DAT` | Datos JSON | Objeto Texto | 8 | |

## 🔍 Índices

| Identificador | Nombre | Tipo de indice |
| :--- | :--- | :--- |
| `ID` | Código | Clave única |
| `CFDI_CPLM` | CFDI | Acepta repetidas |

## 📝 Notas
- El campo `JSN_DAT` permite almacenar la información específica de cada complemento en formato JSON para mayor flexibilidad.
- El campo `TIP` identifica el nombre técnico del complemento del SAT.
