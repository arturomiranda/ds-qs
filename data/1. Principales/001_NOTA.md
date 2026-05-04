# Tabla: 001: Notas

Esta tabla permite adjuntar comentarios, anotaciones o notas de seguimiento a cualquier entidad.

## 📄 Información General
- **ID de Tabla:** `001_NOTA`
- **Tipo:** Maestro
- **Reside en:** Disco
- **Longitud del registro:** 280
- **Número de campos:** 8
- **Número de índices:** 3
- **Descripción:** Repositorio de notas y comentarios vinculados a entidades.

## 🛠️ Estructura de Campos

| Identificador | Nombre | Tipo | Longitud | Tipo de enlace |
| :--- | :--- | :--- | :--- | :--- |
| `ID` | Código | Numérico | 3 | |
| `ENT_NOT` | Notas de Entidad | Numérico | 3 | Maestro: 001_ENT@datta_erp_dat |
| `CON_NOTA` | Contenido de la nota | Alfa 256 | 256 | |
| `FECH` | Fecha | Fecha | 3 | |
| `CRD_USR` | Creado por | Numérico | 3 | Maestro: 001_USU@datta_erp_dat |
| `CRD_TIM` | Creado el | Tiempo | 4 | |
| `MDF_USR` | Modificado por | Numérico | 3 | Maestro: 001_USU@datta_erp_dat |
| `MDF_TIM` | Modificado el | Tiempo | 4 | |

## 🔍 Índices

| Identificador | Nombre | Tipo de indice |
| :--- | :--- | :--- |
| `ID` | Código | Clave única |
| `REL_ENT` | Por Entidad | Acepta repetidas |
| `FEC_ORD` | Orden Cronológico | Acepta repetidas |

## 📝 Notas
- El campo `CON_NOTA` almacena el texto descriptivo de la nota.
- Incluye trazabilidad completa (`CRD_USR`, `MDF_USR`, etc.) para saber quién creó o modificó el comentario.
