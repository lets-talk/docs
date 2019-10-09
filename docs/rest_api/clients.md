# Clientes
## Obtener información de un cliente

<!-- panels:start -->

<!-- div:right-panel -->

<!-- tabs:start -->
#### ** Request **

```shell
curl --location --request GET "https://api.letsta.lk/api/v1/clients/7089264" \
--header "Authorization: Basic Q0V6cHJ0M2R6ckdHeGV6eHpob2Q6WA==" \
--header "Content-Type: application/json"
```
<!-- tabs:end -->

?> El comando previo retorna un JSON como el siguiente:

<!-- tabs:start -->
#### ** Response **

```json
{
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
```
<!-- tabs:end -->

<!-- div:left-panel -->

Obtención de la información de ***usuario/cliente***. Solo es posible obtener la información del ***usuario/cliente*** autenticado.



### HTTP Request

`GET https://api.letsta.lk/api/v1/clients/{{client_id}}`

!>Este <i>endpoint</i> solo puede ser utilizado por el tipo <strong>usuario/cliente</strong>.

### Query Parameters
Sin parámetros.

### Códigos de respuesta

| Código | Significado                    |
|--------|--------------------------------|
| 401    | Credenciales inválidas.        |
| 200    | Operación ejecutada con éxito. |


<!-- panels:end -->
