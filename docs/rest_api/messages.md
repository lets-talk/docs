# Mensajes
## Crear mensaje de texto plano

<!-- panels:start -->

<!-- div:right-panel -->

<!-- tabs:start -->
#### ** Request **

```shell
curl --location --request POST "https://api.letsta.lk/api/v1/messages" \
--header "Authorization: Basic {{encoded_api_key}}" \
--header "Content-Type: application/json" \
--data "{
  \"content\": \"Quiero comprar un Molecular Disruption Device\",
  \"content_type\": \"text/plain\",
  \"remote_id\": \"a55ac7b8-bca7-4ab3-a2e0-e0579fa21cea\",
  \"conversation_id\": 1483229
}"
```
<!-- tabs:end -->

?> El comando previo retorna un JSON como el siguiente:

<!-- tabs:start -->
#### ** Response **

```json
{
    "id": 32478646,
    "content_type": "text/plain",
    "content": "Quiero comprar un Molecular Disruption Device",
    "who": "client",
    "replied_on": "2019-10-04T14:37:22.000+0000",
    "mtype": "NORMAL",
    "subject": null,
    "created_at": "2019-10-04T14:37:22.000+0000",
    "conversation_id": 1483229,
    "remote_id": "a55ac7b8-bca7-4ab3-a2e0-e0579fa21cea",
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
```
<!-- tabs:end -->

<!-- div:left-panel -->

Publicación de un mensaje de texto plano a una conversación. El autor del mensaje es el usuario autenticado con la *API Key* especificada en el *header*.


!> Este <i>endpoint</i> puede ser utilizado tanto por <strong>usuario/cliente</strong> como por <strong>usuario/agente</strong>.


### HTTP Request

`POST https://api.letsta.lk/api/v1/messages`


### Body Parameters


| Propiedad       | Tipo    | ¿Requerido? | Valores permitidos | Descripción                                                                                                                                                   |
|-----------------|---------|-------------|--------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------|
| content         | string  | si          | -                  | Contenido del mensaje.                                                                                                                                        |
| content_type    | string  | si          | `text/plain`       | Tipo de mensaje.                                                                                                                                              |
| remote_id       | object  | si          | -                  | Identificador del mensaje creado por el consumidor de la *API*. Debe seguir el formato [GUID](https://es.wikipedia.org/wiki/Identificador_%C3%BAnico_global). |
| conversation_id | integer | si          | -                  | Id de conversación en donde se publicará el mensaje.                                                                                                          |



### Códigos de respuesta

| Código | Significado                    |
|--------|--------------------------------|
| 401    | Credenciales inválidas.        |
| 200    | Operación ejecutada con éxito. |
| 201    | Creación exitosa del recurso. |
| 404    | El recurso referenciado no existe. |
| 403    | Bloqueo de IP o solicitud no autorizada |


<!-- panels:end -->
## Crear mensaje con archivo adjunto

<!-- panels:start -->

<!-- div:right-panel -->

<!-- tabs:start -->
#### ** Request **

```shell
curl --location --request POST "https://api.letsta.lk/api/v1/messages" \
--header "Authorization: Basic {{encoded_api_key}}" \
--form "content=attachment" \
--form "content_type=image/png" \
--form "conversation_id=1484305" \
--form "remote_id=17bdd605-a88b-4fda-927a-dd739c7209a0"
```
<!-- tabs:end -->

?> El comando previo retorna un JSON como el siguiente:

<!-- tabs:start -->
#### ** Response **

```json
{
    "id": 32497317,
    "content_type": "image/png",
    "content": "attachment",
    "who": "client",
    "replied_on": "2019-10-04T17:17:09.000+0000",
    "mtype": "NORMAL",
    "subject": null,
    "created_at": "2019-10-04T17:17:09.000+0000",
    "conversation_id": 1484305,
    "remote_id": "17bdd605-a88b-4fda-927a-dd739c7209a0",
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
```
<!-- tabs:end -->

<!-- div:left-panel -->

Publicación de un mensaje con archivo adjunto. El autor del mensaje es el cliente autenticado con la *API Key* especificada en el *header*.


!> Este <i>endpoint</i> solo puede ser utilizado por el tipo <strong>usuario/cliente</strong> y <strong>usuario/agente</strong>.


### HTTP Request

`POST https://api.letsta.lk/api/v1/messages`


### Form Parameters


| Parameter    | Tipo   | ¿Requerido? | Valores permitidos                   | Descripción                                                                                                                                                   |
|--------------|--------|-------------|--------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------|
| content      | string | si          | `attachment`                         | Tipo de mensaje.                                                                                                                                              |
| content_type | string | si          | [Valores permitidos](#valores-permitidos-de-content_type) | Tipo de archivo adjunto.                                                                                                                                      |
| media        | file   | si          | -                                    | Archivo adjunto.                                                                                                                                              |
| remote_id    | object | si          | -                                    | Identificador del mensaje creado por el consumidor de la *API*. Debe seguir el formato [GUID](https://es.wikipedia.org/wiki/Identificador_%C3%BAnico_global). |


<!-- panels:end -->
## Crear mensaje con tipo actionable/list

<!-- panels:start -->

<!-- div:right-panel -->

<!-- tabs:start -->
#### ** Request **

```shell
curl --location --request POST "https://api.letsta.lk/api/v1/messages" \
--header "Authorization: Basic {{encoded_api_key}}" \
--header "Content-Type: application/json" \
--data "{
  \"content\": \"{
    \"title\": \"Eliga una opcion:\",
    \"options\": {
        \"viewType\": \"pill\",
        \"showItemsIcon\": true,
    },
    \"items\": [
        {\"type\": \"link\", \"label\": \"Haz click\", \"url\": \"https://www.google.com\", \"target\": \"_blank\"},
        {\"type\": \"quick-reply\", \"label\": \"Transferirme\", \"message\": \"Transferirme con un ejecutvio\"},
    ]
  }\",
  \"content_type\": \"actionable/list\",
  \"remote_id\": \"a55ac7b8-bca7-4ab3-a2e0-e0579fa21cea\",
  \"conversation_id\": 1483229
}"
```
<!-- tabs:end -->

?> El comando previo retorna un JSON como el siguiente:

<!-- tabs:start -->
#### ** Response **

```json
{
    "id": 32497317,
    "content_type": "application/json",
    "content": "{
        \"title\": \"Eliga una opcion:\",
        \"options\": {
            \"viewType\": \"pill\",
            \"showItemsIcon\": true,
        },
        \"items\": [
            {\"type\": \"link\", \"label\": \"Haz click\", \"url\": \"https://www.google.com\", \"target\": \"_blank\"},
            {\"type\": \"quick-reply\", \"label\": \"Transferirme\", \"message\": \"Transferirme con un ejecutvio\"},
        ]
    }",
    "who": "client",
    "replied_on": "2019-10-04T17:17:09.000+0000",
    "mtype": "NORMAL",
    "subject": null,
    "created_at": "2019-10-04T17:17:09.000+0000",
    "conversation_id": 1484305,
    "remote_id": "a55ac7b8-bca7-4ab3-a2e0-e0579fa21cea",
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
```
<!-- tabs:end -->

<!-- div:left-panel -->

Publicación de un mensaje con archivo adjunto. El autor del mensaje es el cliente autenticado con la *API Key* especificada en el *header*.


!> Este <i>endpoint</i> solo puede ser utilizado por el tipo <strong>usuario/cliente</strong> y <strong>usuario/agente</strong>.


### HTTP Request

`POST https://api.letsta.lk/api/v1/messages`


### Form Parameters

| Propiedad       | Tipo    | ¿Requerido? | Valores permitidos | Descripción                                                                                                                                                    |
|-----------------|---------|-------------|--------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------|
| content         | string  | si          | -                  | JSON object con el contenido del mensaje. Debe tener los campos: title, items                 |
| content_type    | string  | si          | `actionable/list`  | Tipo mensaje.                                                                                 |
| remote_id       | object  | si          | -                  | Identificador del mensaje creado por el consumidor de la *API*. Debe seguir el formato [GUID](https://es.wikipedia.org/wiki/Identificador_%C3%BAnico_global). |
| conversation_id | integer | si          | -                  | Id de conversación en donde se publicará el mensaje.  

### Formato del content

| Propiedad       | Tipo    | ¿Requerido? | Valores permitidos | Descripción                                                                                                                                                    |
|-----------------|---------|-------------|--------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------|
| title           | string  | si          | -                  | El texto que se muestra como titulo de las diferentes opciones (se muestra como un mensaje sobre la opciones)                                                      |
| options         | object  | No          | ActionableOptions  | Opciones del mensaje                                                                          |
| items           | array   | si          | ObjectItemType     | Objecto que representa el tipo de opcion                                                      |

#### Tipos de items permitidos (ObjectItemType)

- link

| Propiedad       | Tipo    | ¿Requerido? | Valores permitidos Descripción|
|-----------------|---------|-------------|--------------------|------------|
| type            | string  | si          | `link`             | El tipo de esta acción                                                          |
| payload         | object  | si          | OptionLinkPayload  | Opciones de la opción de link

- quick-reply

| Propiedad       | Tipo    | ¿Requerido? | Valores permitidos | Descripción|
|-----------------|---------|-------------|--------------------|------------|
| type            | string  | si          | `quick-reply`      | El tipo de esta acción                                             |
| payload         | object  | si          | OptionLinkPayload  | Opciones de la opción de link                                      |

#### Tipo de opciones (ActionableOptions)

- link

| Propiedad       | Tipo    | ¿Requerido? | Valores permitidos | Valor por defecto | Descripción                                                               |
|-----------------|---------|-------------|--------------------|-----------------------------------------------------------------------------------------------|
| viewType        | string  | si          | `list` | `pill`    | `list`            | El tipo de item elements                                                   |
| showItemsIcon   | string  | si          | Boolean            | false             | Mostrar iconos de opciones                                                |

### `payload` field de type `link` (OptionLinkPayload)
| Propiedad       | Tipo    | ¿Requerido? | Valores permitidos Descripción|
|-----------------|---------|-------------|-------------------------------|
| label           | string  | si          | -                  | Texto que se usa para desplegar                                                 |
| url             | string  | si          | -                  | Url absoluta del link que se quiere abrir                                       |
| target          | string  | no          | `_blank`, `_parent`| Determina donde se abre el link, si en una pagina nueva o en la pagina padre    |

### `payload` field de type `quick-reply` (QuickReplyPayload)
| Propiedad       | Tipo    | ¿Requerido? | Valores permitidos Descripción|
|-----------------|---------|-------------|-------------------------------|
| label           | string  | si          | -                  | Texto que se usa para desplegar                                    |
| message         | string  | si          | -                  | Texto del mensaje que se escribe cuando se selecciona esta opción  |


#### Valores permitidos de `content_type`

Los valores permitidos pueden estar restringidos por configuración de la organización y corresponden a un subconjunto de los siguientes:

| Content Type                                                                | Descripción                                     |
|-----------------------------------------------------------------------------|-------------------------------------------------|
| `application/csv`                                                           | Valores separados por coma (CSV)                |
| `application/geojson`                                                       | Sistema de información geográfica               |
| `application/msword`                                                        | Microsoft Word                                  |
| `application/octet-stream`                                                  | Cualquier tipo de datos binarios                |
| `application/pdf`                                                           | Adobe Portable Document Format (PDF)            |
| `application/vnd.ms-excel`                                                  | Microsoft Excel                                 |
| `application/vnd.ms-powerpoint`                                             | Microsoft PowerPoint                            |
| `application/vnd.openxmlformats` <br> `-officedocument.presentationml.presentation` | Microsoft PowerPoint (PPTX)                     |
| `application/vnd.openxmlformats` <br> `-officedocument.spreadsheetml.sheet`         | Microsoft Excel (XLSX)                          |
| `application/vnd.openxmlformats` <br> `-officedocument.wordprocessingml.document`   | Microsoft Word (DOCX)                           |
| `audio/mp3`                                                                 | MPEG-1 Audio Layer III o MPEG-2 Audio Layer III |
| `image/bmp`                                                                 | Imagen BITMAP                                   |
| `image/gif`                                                                 | Graphics Interchange Format (GIF)               |
| `image/jpeg`                                                                | Imágenes JPEG                                   |
| `image/jpg`                                                                 | Imágenes JPEG                                   |
| `image/png`                                                                 | Imágenes PNG                                    |
| `image/tiff`                                                                | Formato de archivo de imagen etiquetado (TIFF)  |
| `image/x-tiff`                                                              | Formato de archivo de imagen etiquetado (TIFF)  |
| `text/comma-separated-values`                                               | Valores separados por coma (CSV)                |
| `text/csv`                                                                  | Valores separados por coma (CSV)                |
| `video/mov`                                                                 | QuickTime                                       |
| `video/mp4`                                                                 | MPEG-4                                          |
| `video/quicktime`                                                           | QuickTime                                       |
| `actionable/list`                                                           | Actionable List opciones de distintos tipos     |

strip_emoji
### Códigos de respuesta

| Código | Significado                    |
|--------|--------------------------------|
| 401    | Credenciales inválidas.        |
| 200    | Operación ejecutada con éxito. |
| 201    | Creación exitosa del recurso. |
| 404    | El recurso referenciado no existe. |
| 403    | Bloqueo de IP o solicitud no autorizada |


<!-- panels:end -->


## Obtener mensajes de conversación

<!-- panels:start -->

<!-- div:right-panel -->

<!-- tabs:start -->
#### ** Request **

```shell
curl --location --request GET "https://api.letsta.lk/api/v1/messages?conversation_id=1484305&cursor[type]=some_after&cursor[location]=32496705&page=1&cursor[amount]=2" \
--header "Authorization: Basic {{encoded_api_key}}"
```
<!-- tabs:end -->

?> El comando previo retorna un JSON como el siguiente:

<!-- tabs:start -->
#### ** Response **

```json
[
    {
        "id": 32506061,
        "subject": null,
        "content": "kk",
        "type": "NORMAL",
        "created_at": "2019-10-04T19:04:32.000+0000",
        "content_type": "text/plain",
        "conversation_id": 1484305,
        "remote_id": "ef55489a-cb1f-4bc4-8f91-23d201865670",
        "internal": false,
        "replied_on": "2019-10-04T19:04:32.000+0000",
        "conversation_type_name": "normal",
        "conversation_internal_issue": "hola",
        "client_name": "Andrew Wiggin",
        "client_avatar": "https://api.letsta.lk/assets/default-avatar.gif",
        "conversation_status": "Open",
        "person_type": "Client",
        "person_id": 7089264,
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
    {
        "id": 32505934,
        "subject": null,
        "content": "sd",
        "type": "NORMAL",
        "created_at": "2019-10-04T19:02:58.000+0000",
        "content_type": "text/plain",
        "conversation_id": 1484305,
        "remote_id": "6aa8e7f3-389f-4117-a303-c631b3c4f27f",
        "internal": false,
        "replied_on": "2019-10-04T19:02:58.000+0000",
        "conversation_type_name": "normal",
        "conversation_internal_issue": "hola",
        "client_name": "Andrew Wiggin",
        "client_avatar": "https://api.letsta.lk/assets/default-avatar.gif",
        "conversation_status": "Open",
        "person_type": "Client",
        "person_id": 7089264,
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
]
```
<!-- tabs:end -->

<!-- div:left-panel -->

Obtención de los mensajes de una conversación.

!> Este <i>endpoint</i> puede ser utilizado tanto por <strong>usuario/cliente</strong> como por <strong>usuario/agente</strong>.


### HTTP Request

`GET https://api.letsta.lk/api/v1/messages`

### Query Parameters

| Parámetro        | ¿Requerido?      | Valores permitidos                                       | Descripción                                                    |
|------------------|------------------|----------------------------------------------------------|----------------------------------------------------------------|
| conversation_id  | si               | -                                                        | Id de la conversación de los cuales se solicitan los mensajes. |
| cursor[type]     | condicionalmente | [Valores permitidos](#valores-permitidos-de-cursor-type) | Tipo de cursor a usar en la paginación.                        |
| cursor[location] | condicionalmente | -                                                        | Identificador que indica a partir de que recurso se paginará.  |
| cursor[amount]   | condicionalmente | -                                                        | Cantidad de recursos por página a listar.                      |

#### Valores permitidos de cursor[type]

| Valor         | Descripción                                                                                            |
|---------------|--------------------------------------------------------------------------------------------------------|
| `some_after`  | Retorna la cantidad de elementos especificada por `cursor[amount]`  **después** de `cursor[location]`. |
| `some_before` | Retorna la cantidad de elementos especificada por `cursor[amount]`  **antes** de `cursor[location]`.   |



### Códigos de respuesta

| Código | Significado                    |
|--------|--------------------------------|
| 401    | Credenciales inválidas.        |
| 200    | Operación ejecutada con éxito. |
| 201    | Creación exitosa del recurso. |
| 404    | El recurso referenciado no existe. |
| 403    | Bloqueo de IP o solicitud no autorizada. |


<!-- panels:end -->
