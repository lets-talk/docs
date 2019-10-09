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
