# Conversaciones
## Crear conversación

<!-- panels:start -->

<!-- div:right-panel -->

<!-- tabs:start -->
#### ** Request **

```shell
curl --location --request POST "https://api.letsta.lk/api/v1/conversations" \
--header "Content-Type: application/json" \
--header "Authorization: Basic {{encoded_client_api_key}}" \
--data "{
  \"issue\": \"Quiero un crédito para combatir a los fórmicos\",
  \"message\": {
    \"content\": \"Quiero un crédito para combatir a los fórmicos\",
    \"content_type\": \"text/plain\",
    \"remote_id\": \"ec7100f2-b9a8-4e51-bab5-6f52e727c763\"
  },
  \"organization_id\": 171,
  \"inquiry_id\": 667,
  \"metadata\": {
    \"page\": \"https://escueladebatalla.com\"
  }
}"
```
<!-- tabs:end -->

?> El comando previo retorna un JSON como el siguiente:

<!-- tabs:start -->
#### ** Response **

```json
{
    "id": 1483053,
    "issue": "Quiero un crédito para combatir a los fórmicos",
    "internal_issue": "Quiero un crédito para combatir a los fórmicos",
    "geo": null,
    "status": "Open",
    "client": {
        "id": 7089264,
        "name": "Andrew Wiggin",
        "email": "andrew.wiggin@letsta.lk",
        "created_at": "2019-10-03T19:10:15.000+0000",
        "avatar": "https://api.letsta.lk/assets/default-avatar.gif",
        "client_key_values": [
            {
                "id": 10915064,
                "key": "name",
                "value": "Andrew Wiggin"
            },
            {
                "id": 10915065,
                "key": "email",
                "value": "andrew.wiggin@letsta.lk"
            }
        ],
        "phone": null,
        "avatar_file_name": "https://api.letsta.lk/assets/default-avatar.gif",
        "confirmed": true,
        "guest": false,
        "token": null,
        "type": "Client"
    },
    "created_at": "2019-10-04T11:41:38.000+0000",
    "type": "normal",
    "unread_messages": 0,
    "avatar_client": "https://api.letsta.lk/assets/default-avatar.gif",
    "response_server_time": "2019-10-04T11:41:38.000+0000",
    "involved_persons": [
        {
            "id": 7089264,
            "name": "Andrew Wiggin",
            "email": "andrew.wiggin@letsta.lk",
            "created_at": "2019-10-03T19:10:15.000+0000",
            "avatar": "https://s3.amazonaws.com/prod.letsta.lk/clients/avatars/e80cfc21ceb2aacc6ffad265e252f57759f44a04/small.gif",
            "client_key_values": [
                {
                    "id": 10915064,
                    "key": "name",
                    "value": "Andrew Wiggin"
                },
                {
                    "id": 10915065,
                    "key": "email",
                    "value": "andrew.wiggin@letsta.lk"
                }
            ],
            "phone": null,
            "avatar_file_name": "https://s3.amazonaws.com/prod.letsta.lk/clients/avatars/1015396405130a35463fe654b4982c17e5d9b0a9/thumb.gif",
            "confirmed": true,
            "guest": false,
            "token": null,
            "type": "Client"
        }
    ],
    "organization_id": 171,
    "new_messages": [
        {
            "id": 32462919,
            "content_type": "text/plain",
            "content": "Quiero un crédito para combatir a los fórmicos",
            "who": "client",
            "replied_on": "2019-10-04T11:41:38.000+0000",
            "mtype": "NORMAL",
            "subject": null,
            "created_at": "2019-10-04T11:41:38.000+0000",
            "conversation_id": 1483053,
            "remote_id": "ec7100f2-b9a8-4e51-bab5-6f52e727c763",
            "internal": false,
            "type": "NORMAL",
            "person": {
                "id": 7089264,
                "name": "Andrew Wiggin",
                "internal_name": null,
                "avatar": "https://api.letsta.lk/assets/default-avatar.gif",
                "type": "Client",
                "email": "andrew.wiggin@letsta.lk",
                "active": true,
                "description": null,
                "role": null,
                "availability_status_name": null,
                "status_name": null
            }
        }
    ],
    "last_message": {
        "id": 32462919,
        "content_type": "text/plain",
        "content": "Quiero un crédito para combatir a los fórmicos",
        "who": "client",
        "replied_on": "2019-10-04T11:41:38.000+0000",
        "mtype": "NORMAL",
        "subject": null,
        "created_at": "2019-10-04T11:41:38.000+0000",
        "conversation_id": 1483053,
        "remote_id": "ec7100f2-b9a8-4e51-bab5-6f52e727c763",
        "internal": false,
        "type": "NORMAL",
        "person": {
            "id": 7089264,
            "name": "Andrew Wiggin",
            "internal_name": null,
            "avatar": "https://api.letsta.lk/assets/default-avatar.gif",
            "type": "Client",
            "email": "andrew.wiggin@letsta.lk",
            "active": true,
            "description": null,
            "role": null,
            "availability_status_name": null,
            "status_name": null
        }
    },
    "token": null,
    "updater_type": null,
    "updater_id": null,
    "rate": "neutral",
    "type_name": "normal",
    "title": null,
    "project_list": [],
    "greeting": "Hola!! como estás"
}
```
<!-- tabs:end -->

<!-- div:left-panel -->

Creación de una una conversación. El autor del mensaje y la conversación es el cliente autenticado con la *API Key* especificada en el *header*.

!> Este <i>endpoint</i> solo puede ser utilizado por el tipo <strong>usuario/cliente</strong>.

### HTTP Request

`POST https://api.letsta.lk/api/v1/conversations`

### Body Parameters

| Propiedad           | Tipo    | ¿Requerido? | Descripción                                                          |
|---------------------|---------|-------------|----------------------------------------------------------------------|
| issue               | string  | no          | Título de la conversación. Si no se especifica, la conversación tendrá por titulo `Conversation [Id]`.                                          |
| [message](#message) | objeto  | si          | Mensaje de la conversación                                           |
| organization_id     | integer | si          | Id de la organización donde se creará la conversación                |
| inquiry_id          | integer | no          | Id del inquiry de la conversación                                    |
| metadata            | objecto | no          | Metadata asociada a la conversación. Es un objeto JSON de uso libre. |

#### message

| Propiedad    | Tipo   | ¿Requerido? | Descripción                                                                                                                                                  | Valor aceptado        |
|--------------|--------|-------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------|--------------|
| content      | string | si          | contenido del mensage                                                                                                                                        | -            |
| content_type | string | si          | tipo de mensage                                                                                                                                              | `text/plain` |
| remote_id    | string | si          | Identificador del mensaje creado por el consumidor de la *API*. Debe seguir el formato [GUID](https://es.wikipedia.org/wiki/Identificador_%C3%BAnico_global) | -            |


[filename](../rest_api/_response_codes.md ':include')

<!-- panels:end -->

## Cerrar conversación

<!-- panels:start -->

<!-- div:right-panel -->

<!-- tabs:start -->
#### ** Request **

```shell
curl --location --request PUT "https://pingpong.letsta.lk/api/v1/conversations/1483229" \
--header "Authorization: Basic {{encoded_agent_api_key}}" \
--header "Content-Type: application/json" \
--data "{\"status\":\"Closed\"}"
```
<!-- tabs:end -->

?> El comando previo retorna un JSON como el siguiente:

<!-- tabs:start -->
#### ** Response **

```json
{
    "id": 1483229,
    "issue": "--",
    "internal_issue": "--",
    "status": "Closed",
    "created_at": "2019-10-04T12:48:05.000+0000",
    "involved_persons": [
        {
            "id": 4322,
            "name": "Admin",
            "internal_name": null,
            "avatar": "https://s3.amazonaws.com/prod.letsta.lk/users/avatars/ded11e3d2a9ff29e8c3fc50ec4180c6b3fb7eff7/thumb.gif",
            "type": "User",
            "email": "admin.pingpong@letsta.lk",
            "active": true,
            "description": null,
            "role": "admin",
            "availability_status_name": "active",
            "status_name": "online"
        },
        {
            "id": 7089264,
            "name": "Andrew Wiggin",
            "internal_name": null,
            "avatar": "https://s3.amazonaws.com/prod.letsta.lk/clients/avatars/1015396405130a35463fe654b4982c17e5d9b0a9/thumb.gif",
            "type": "Client",
            "email": "andrew.wiggin@letsta.lk",
            "active": true,
            "description": null,
            "role": null,
            "availability_status_name": null,
            "status_name": null
        }
    ],
    "organization_id": 171,
    "token": "uhh1XE2XaXrf7dSyC3l8AQ",
    "updater_type": "User",
    "updater_id": 4322,
    "changes": null,
    "geo": null,
    "rate": "neutral",
    "rate_class": null,
    "inquiry_name": "Sales",
    "tag_list": [],
    "last_message": {
        "id": 32584874,
        "content_type": "text/plain",
        "content": "closed",
        "who": "user",
        "replied_on": "2019-10-07T15:04:39.000+0000",
        "mtype": "SYSTEM",
        "subject": null,
        "created_at": "2019-10-07T15:04:39.000+0000",
        "conversation_id": 1483229,
        "remote_id": null,
        "internal": false,
        "type": "SYSTEM",
        "person": {
            "id": 4322,
            "name": "Admin",
            "internal_name": null,
            "avatar": "https://api.letsta.lk/assets/default-avatar.gif",
            "type": "User",
            "email": "admin.pingpong@letsta.lk",
            "active": true,
            "description": null,
            "role": "admin",
            "availability_status_name": "active",
            "status_name": "online"
        }
    },
    "unread_messages": 50,
    "ctype": 0,
    "type_name": "normal",
    "title": "Andrew Wiggin",
    "rate_type": {
        "value": null,
        "options": {
            "min": 1,
            "max": 2
        },
        "type": "Binary"
    },
    "granted": true,
    "project_list": [
        "test"
    ],
    "subscribe_token": "DBKaBGqcnjPhwFwaaW5L",
    "greeting": "Hola.. soy Super Ventas!!! le vendo hielo a los esquimales",
    "extra_data": {},
    "closed_at": "2019-10-07T15:04:39.000+0000",
    "groups": [],
    "client": {
        "id": 7089264,
        "name": "Andrew Wiggin",
        "email": "andrew.wiggin@letsta.lk",
        "created_at": "2019-10-03T19:10:15.000+0000",
        "avatar": "https://api.letsta.lk/assets/default-avatar.gif",
        "client_key_values": [
            {
                "id": 10915064,
                "key": "name",
                "value": "Andrew Wiggin"
            },
            {
                "id": 10915065,
                "key": "email",
                "value": "andrew.wiggin@letsta.lk"
            }
        ],
        "phone": null,
        "avatar_file_name": "https://api.letsta.lk/assets/default-avatar.gif",
        "confirmed": true,
        "guest": false,
        "token": null,
        "type": "Client"
    }
}
```
<!-- tabs:end -->

<!-- div:left-panel -->

Cierre de una conversación.

!> Este <i>endpoint</i> solo puede ser utilizado por el tipo <strong>usuario/agente</strong>.

### HTTP Request

`PUT https://pingpong.letsta.lk/api/v1/conversations/:id`

### Query Parameters

| Parámetro | ¿Requerido? | Descripción                                |
|-----------|-------------|--------------------------------------------|
| id        | si          | Identificador de la conversación a cerrar. |

### Body Parameters

| Propiedad | Tipo   | ¿Requerido? | Valores permitidos | Descripción                             |
|-----------|--------|-------------|--------------------|-----------------------------------------|
| status    | string | si          | `Closed`           | Estado a actualizar de la conversación. |


[filename](../rest_api/_response_codes.md ':include')

<!-- panels:end -->

