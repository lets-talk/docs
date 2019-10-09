# Tokens
## Crear/obtener un cliente

<!-- panels:start -->

<!-- div:right-panel -->

<!-- tabs:start -->
#### ** Request **

```shell
curl --location --request POST "https://api.letsta.lk/api/v1/integration/auth/token" \
--header "Content-Type: application/json" \
--data "{
   \"email\":\"andrew.wiggin@letsta.lk\",
   \"name\":\"Andrew Wiggin\",
   \"consumer\":{
      \"key\":\"[consumer_key]\",
      \"token\":\"[consumer_token]\"
   },
   \"organization_id\":{organization_id}
}"
```
<!-- tabs:end -->

?> El comando previo retorna un JSON como el siguiente:

<!-- tabs:start -->
#### ** Response **

```json
{
    "token": "[client_token]",
    "person": {
        "id": 7089264,
        "name": "Andrew Wiggin",
        "external_name": null,
        "person_status": null,
        "avatar": "https://api.letsta.lk/assets/default-avatar.gif",
        "avatar_file_name": null,
        "client_key_values": [
            {
                "client_id": 7089264,
                "clients_organization_id": 3320982,
                "created_at": "2019-10-03T19:10:16Z",
                "id": 10915064,
                "key": "name",
                "updated_at": "2019-10-03T19:10:16Z",
                "value": "Andrew Wiggin"
            },
            {
                "client_id": 7089264,
                "clients_organization_id": 3320982,
                "created_at": "2019-10-03T19:10:16Z",
                "id": 10915065,
                "key": "email",
                "updated_at": "2019-10-03T19:10:16Z",
                "value": "andrew.wiggin@letsta.lk"
            }
        ],
        "created_at": "2019-10-03T19:10:15Z",
        "email": "andrew.wiggin@letsta.lk",
        "type": "Client",
        "organization_id": 171,
        "roles": null,
        "owner": null,
        "conversations_feed_order": false,
        "organization_logo": "https://api.letsta.lk/assets/default-avatar.gif",
        "backup_active": null,
        "backup_email": null,
        "backup_stay_in": null,
        "organization_name": "Ping Pong",
        "organization_subdomain": "pingpong",
        "description": null,
        "granted": false
    },
    "permissions_map": null,
    "subdomain": "api",
    "environment": "production",
    "organization_timezone_offset": -14400
}
```
<!-- tabs:end -->

<!-- div:left-panel -->

Creación de un **usuario/cliente** con obtención de token y datos asociados. Si el cliente existe previamente se retornan sus datos existentes.


### HTTP Request

`POST https://api.letsta.lk/api/v1/integration/auth/token`

### Body Parameters


| Propiedad             | Tipo    | ¿Requerido? | Descripción                                                            |
|-----------------------|---------|-------------|------------------------------------------------------------------------|
| email                 | string  | si          | Correo del cliente.                                                    |
| name                  | string  | si          | Nombre del cliente.                                                    |
| [consumer](#consumer) | object  | si          | Objeto para autorizar la creación o obtención del cliente y sus datos. |
| organization_id       | integer | si          | Id de la organización.                                                 |

#### consumer

| Propiedad | Tipo   | ¿Requerido? | Descripción                                                                  |
|-----------|--------|-------------|------------------------------------------------------------------------------|
| key       | string | si          | Consumer key para autorizar la creación o obtención del cliente y sus datos. |
| token     | string | si          | Consumer token para autorizar la creación o obtención del cliente y sus datos. |


### Códigos de respuesta

| Código | Significado                    |
|--------|--------------------------------|
| 401    | Credenciales inválidas.        |
| 200    | Operación ejecutada con éxito. |


<!-- panels:end -->
