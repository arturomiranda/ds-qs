Tipo: Maestro
Número de campos: 16

## 🛠️ Estructura de Campos

| Identificador          | Nombre           | Tipo               | Longitud | Tipo de enlace | Descripción    |
| :--------------------- | :--------------- | :----------------- | :------- | -------------- | :------------- |
| `id`                   | Código           | Numérico (BigInt)  | 10       |                |                |
| `nombres`              | Nombre(s)        | Alfa 256 (Varchar) | 100      |                |                |
| `apellido_paterno`     | Apellido Paterno | Alfa 256 (Varchar) | 100      |                |                |
| `apellido_materno`     | Apellido Materno | Alfa 256 (Varchar) | 100      |                |                |
| `empresa`              | Empresa          | Alfa 256 (Varchar) | 100      |                |                |
| `rfc`                  | RFC Fiscal       | Alfa 256 (Varchar) | 13       |                |                |
| `correo`               | Email            | Alfa 256 (Varchar) | 150      |                | Unique         |
| `telefono`             | Teléfono         | Alfa 256 (Varchar) | 20       |                |                |
| `nombre_usuario`       | Username         | Alfa 256 (Varchar) | 50       |                | Unique         |
| `contrasena_hash`      | Hash Contraseña  | Alfa 256 (Varchar) | 255      |                |                |
| `id_rol`               | Nivel de Acceso  | Numérico (TinyInt) | 2        | FK: Roles      |                |
| `fecha_registro`       | Creado el        | Timestamp          | -        |                | Default: Now() |
| `es_verificado`        | Está Verificado  | Boolean (TinyInt)  | 1        |                | Default: 0     |
| `codigo_otp`           | OTP              | Alfa 256 (Varchar) | 6        |                |                |
| `fecha_expiracion_otp` | Expira OTP       | Datetime           | -        |                |                |
| `forzar_cambio_clave`  | Forzar Reset     | Boolean (TinyInt)  | 1        |                | Default: 1     |
