# Metadata
## Agregar metadata a una conversación

<!-- panels:start -->

<!-- div:right-panel -->

<!-- tabs:start -->
#### ** Request **

```shell
curl --location --request POST "https://api.letsta.lk/api/v1/conversations/{{conversation_id}}/metadata" \
--header "Authorization: Basic {{encoded_api_key}}" \
--header "Content-Type: application/json" \
--data "{
    \"country\":\"Chile\",
    \"IQ\": 160,
    \"uno\":\"1\",
    \"dos\":\"2\"
}
"
```
<!-- tabs:end -->

?> El comando previo retorna un JSON como el siguiente:

<!-- tabs:start -->
#### ** Response **

```json
{
    "country": "Chile",
    "IQ": 160,
    "uno": "1",
    "dos": "2"
}
```
<!-- tabs:end -->

<!-- div:left-panel -->

Adición de metadata en una conversación especificada.


!> Este <i>endpoint</i> puede ser utilizado tanto por <strong>usuario/cliente</strong> como por <strong>usuario/agente</strong>.


### HTTP Request

`PUT https://api.letsta.lk/api/v1/conversations/:id/metadata`

### Query Parameters

| Parámetro | ¿Requerido? | Descripción                                                       |
|-----------|-------------|-------------------------------------------------------------------|
| id        | si          | Identificador de la conversación dónde se adicionará la metadata. |

### Body Parameters

Cualquier objeto JSON que se deseé agregar como metadata


### Códigos de respuesta

| Código | Significado                    |
|--------|--------------------------------|
| 401    | Credenciales inválidas.        |
| 200    | Operación ejecutada con éxito. |


<!-- panels:end -->
## Obtener metadata de una conversación

<!-- panels:start -->

<!-- div:right-panel -->

<!-- tabs:start -->
#### ** Request **
```shell
curl --location --request GET 'https://api.letsta.lk/api/v1/conversations/{{conversation_id}}/metadata' \
--header 'Authorization: Basic {{encoded_api_key}}' \
--header 'Content-Type: application/json'
```
<!-- tabs:end -->

?> El comando previo retorna un JSON como el siguiente:

<!-- tabs:start -->
#### ** Response **

```json
{
    "page": "https://we.letsta.lk/examples/widget?widget_name=widget",
    "platform": "widget",
    "widget_name": "widget",
    "localDate": "Tue Oct 29 2019 18:22:25 GMT-0300 (Chile Summer Time)",
    "empresa": "no-pyme",
    "ip": "186.105.44.27",
    "browser_name": "Chrome",
    "browser_version": "77.0.3865.120",
    "os_name": "Mac",
    "os_version": "10.13.6",
    "device_type": "desktop",
    "location": "Infomation not available"
}
```
<!-- tabs:end -->

<!-- div:left-panel -->

Obtención de la metadata de una conversación

!> Este <i>endpoint</i> puede ser utilizado tanto por <strong>usuario/cliente</strong> como por <strong>usuario/agente</strong>.

### HTTP Request

`GET https://api.letsta.lk/api/v1/conversations/:id/metadata`

### Query Parameters

Ninguno

### Códigos de respuesta

| Código | Significado                    |
|--------|--------------------------------|
| 401    | Credenciales inválidas.        |
| 200    | Operación ejecutada con éxito. |
<!-- panels:end -->
