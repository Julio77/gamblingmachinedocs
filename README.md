# gamblingmachinedocs
Documentacion del API para las maquinas tragamonedas de SparkLife


## Autenticación de Usuario

Este endpoint se utiliza para autenticar a un usuario y proporcionarle acceso mediante un token JWT.

- **Ruta:** `/auth`
- **Método:** POST
- **Parámetros de entrada:** 
  - `login`: Nombre de usuario o dirección de correo electrónico.
  - `pass`: Contraseña del usuario.
- **Respuestas:**
  - `200 OK`: Se proporciona un token JWT válido si la autenticación es exitosa.
    ```json
    [
      {
        "id": "123",
        "name": "John Doe",
        "email": "john@example.com",
        "token": "<token JWT>"
      }
    ]
    ```
  - `401 Unauthorized`: Se devuelve un mensaje de error si la identificación o la contraseña son inválidas.
    ```json
    {
      "message": "Identificación o contraseña inválida"
    }
    ```

### Ejemplo de solicitud:

```http
POST /auth
Content-Type: application/json

{
  "login": "john@example.com",
  "pass": "password123"
}

## Registro de Usuario

Este endpoint se utiliza para registrar un nuevo usuario en el sistema.

- **Ruta:** `/user`
- **Método:** POST
- **Autenticación requerida:** Sí, se requiere autenticación JWT.
- **Parámetros de entrada:** 
  - `name`: Nombre del usuario.
  - `email`: Correo electrónico del usuario.
  - `password`: Contraseña del usuario.
  - (Opcionales) Otros datos del usuario como `address`, `phone`, etc.
- **Respuesta exitosa (200 OK):** Retorna los detalles del usuario recién registrado.
- **Respuesta de error (códigos de estado HTTP 4xx o 5xx):** Mensaje de error en caso de que ocurra un problema durante el registro.

### Ejemplo de solicitud:

```http
POST /user
Authorization: Bearer <token JWT>
Content-Type: application/json

{
  "name": "John Doe",
  "email": "john@example.com",
  "password": "password123"
}


