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

```
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
Content-Type: application/json

{
  "name": "John Doe",
  "email": "john@example.com",
  "password": "password123"
}

HTTP/1.1 200 OK
Content-Type: application/json

{
  "id": "123",
  "name": "John Doe",
  "email": "john@example.com",
  "wallet": "0xabc123def456",
  "nonce": 1234
}

HTTP/1.1 400 Bad Request
Content-Type: application/json

{
  "message": "Error al procesar la solicitud. Por favor, revise los datos proporcionados."
}

```

## Obtener Balance SPS del usuario en el contexto de Máquinas de Juego

Este endpoint se utiliza para obtener el balance en las maquinitas de juego de un usuario específico.

- **Ruta:** `/user/:id/gamblingmachinebalance`
- **Método:** POST
- **Autenticación requerida:** Sí, se requiere autenticación JWT.
- **Parámetros de ruta:** 
  - `id`: Direccion Address (Wallet) del usuario para el cual se desea obtener el balance de la máquina de juego.
- **Respuesta exitosa (200 OK):** Retorna el balance de la máquina de juego del usuario en formato Wei, es decir 1 SPS = 100000000.
- **Respuesta de error (códigos de estado HTTP 4xx o 5xx):** Mensaje de error en caso de que ocurra un problema al obtener el balance.

### Ejemplo de solicitud:

```http
POST /user/0x123abc.../gamblingmachinebalance
x-access-token: <token JWT>

HTTP/1.1 200 OK
Content-Type: application/json

{
  "balance": "1000" // Balance de la máquina de juego del usuario
}

HTTP/1.1 500 Internal Server Error
Content-Type: application/json

{
  "message": "Error al obtener el balance de la máquina de juego. Por favor, inténtalo de nuevo más tarde."
}

```

## Obtener Listado de Máquinas de Juego Activas

Este endpoint se utiliza para obtener el listado de máquinas de juego activas, devolviendo un array con los IDs de los NFTs listados en las máquinas de juego.

- **Ruta:** `/gamblingmachine/active`
- **Método:** GET
- **Autenticación requerida:** Sí, se requiere autenticación JWT.
- **Respuesta exitosa (200 OK):** Retorna un array con los IDs de los NFTs listados en las máquinas de juego activas.
- **Respuesta de error (códigos de estado HTTP 4xx o 5xx):** Mensaje de error en caso de que ocurra un problema al obtener el listado de máquinas de juego activas.

### Ejemplo de solicitud:

```http
GET /gamblingmachine/active
x-access-token: <token JWT>

HTTP/1.1 200 OK
Content-Type: application/json

[
  "1",
  "2",
  "3"
]

HTTP/1.1 500 Internal Server Error
Content-Type: application/json

{
  "message": "Error al obtener el listado de máquinas de juego activas. Por favor, inténtalo de nuevo más tarde."
}
```

## Jugar en la Máquina de Juego

Este endpoint se utiliza para jugar en una máquina de juego específica, moviendo los fondos en el lado del servidor.

- **Ruta:** `/gamblingmachine/:id/play`
- **Método:** POST
- **Autenticación requerida:** Sí, se requiere autenticación JWT.
- **Parámetros de ruta:** 
  - `id`: ID de la máquina de juego en la que se desea jugar (NFT ID).
- **Parámetros de entrada en el cuerpo de la solicitud:** 
  - `player`: Dirección del jugador que está jugando.
  - `serverAddress`: Dirección del servidor que controla la máquina de juego.
  - `newBalance`: Nuevo saldo del jugador después del juego.
- **Respuesta exitosa (200 OK):** Retorna el resultado de la transacción de juego.
- **Respuesta de error (códigos de estado HTTP 4xx o 5xx):** Mensaje de error en caso de que ocurra un problema durante el juego.

### Ejemplo de solicitud:

```http
POST /gamblingmachine/1/play
x-access-token: <token JWT>
Content-Type: application/json

{
  "player": "0xabc123def456",
  "serverAddress": "0x123456789abc",
  "newBalance": "1000"
}

HTTP/1.1 200 OK
Content-Type: application/json

{
  "txHash": "0x123456789",
  "dealerAddress": "0x123456789abc",
  "player": "0xabc123def456",
  "newBalance": "1000"
}

HTTP/1.1 500 Internal Server Error
Content-Type: application/json

{
  "message": "Error al jugar en la máquina de juego. Por favor, inténtalo de nuevo más tarde."
}
```




