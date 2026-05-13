# Tabla: ID: velneo_instancias

## 📄 Información General
- Tipo de tabla: Maestro
- Reside en: Disco
- Longitud del registro: 750
- Número de campos: 9
- Número de índices: 2

## 📝 Descripción [IA]
Catálogo técnico de las cajas VCD (instancias) desplegadas en Velneo Cloud. Esta tabla diferencia entre instancias de datos (DAT) y de aplicación (APP), almacenando sus rutas físicas, alias de proyecto y códigos internos del servidor. Es la tabla de referencia para construir las cadenas de herencia y configuración dinámica necesarias para que el cliente acceda a su software ERP.

## 🛠️ Estructura de Campos

| Identificador | Nombre | Tipo | Longitud | Tipo de enlace | Comentarios y Relaciones |
| :--- | :--- | :--- | :--- | :--- | :--- |
| `id` | ID Tabla | Numérico | 10 | | Clave primaria autoincremental. |
| `id_instancia` | ID Instancia | Alfa | 100 | | Identificador único devuelto por Velneo Cloud (ej. PRUEBA_DAT). |
| `nombre` | Nombre | Alfa | 100 | | Nombre descriptivo asignado a la instancia. |
| `ruta` | Ruta Física | Alfa | 255 | | Path en el vServer (ej. cloud01/proyectos/datta_dat). |
| `proyecto` | Proyecto | Alfa | 100 | | Alias del archivo .vcd vinculado. |
| `solucion` | Solución | Alfa | 100 | | Nombre de la solución Velneo (ej. DATTA ERP). |
| `codigo` | Cód. Servidor | Numérico | 11 | | Identificador numérico interno del vServer. |
| `tipo` | Tipo | Enum | - | | dat, app. Crucial para la lógica de herencia de cajas. |
| `fecha_creacion` | Creada el | Tiempo | 4 | | Timestamp del registro técnico. |

## 🔍 Índices

| Identificador | Nombre | Tipo de índice |
| :--- | :--- | :--- |
| `PRIMARY` | Primary Key | Clave única (id) |
| `uk_id_instancia_tipo` | ID y Tipo Únicos | Clave única (id_instancia, tipo) |

## 🔗 Enlaces Plurales
- **velneo:** Las instancias son referenciadas por la tabla de infraestructura principal.

## 📌 Notas [IA]
- El índice compuesto `uk_id_instancia_tipo` evita que se registren IDs duplicados para el mismo tipo de caja, garantizando la consistencia del despliegue.
- Esta tabla debe mantenerse sincronizada con la realidad del vServer; si una instancia es borrada manualmente en Cloud, este registro debe actualizarse.
