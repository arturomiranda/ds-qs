Tipo: Catálogo / Diccionario
Número de campos: 4

## 🛠️ Estructura de Campos

| Identificador | Nombre         | Tipo               | Longitud | Descripción              | Tipo de enlace |
| :------------ | :------------- | :----------------- | :------- | :----------------------- | -------------- |
| `id`          | Código         | Numérico (TinyInt) | 2        |                          |                |
| `nombre_rol`  | Nombre del Rol | Alfa 256 (Varchar) | 50       | (Ej: 'admin', 'cliente') |                |
| `descripcion` | Alcance        | Alfa 256 (Varchar) | 255      |                          |                |
| `es_activo`   | Estado         | Boolean (TinyInt)  | 1        | Default: 1               |                |
