# Estados personalizados de agente
## Obtener histórico

<!-- panels:start -->

<!-- div:right-panel -->

<!-- tabs:start -->
#### ** Request **

```shell
curl --location --request GET "https://api.letsta.lk/api/v1/custom_status_histories?from=2020-01-28%2012:26&to=2020-01-28%2012:27&page=1" \
--header "Authorization: Basic Q0V6cHJ0M2R6ckdHeGV6eHpob2Q6WA==" \
--header "Content-Type: application/json"
```
<!-- tabs:end -->

?> El comando previo retorna un JSON como el siguiente:

<!-- tabs:start -->
#### ** Response **
```json
{
  "id": 6,
  "start_date": "2019-12-27 23:22:12",
  "end_date": "2019-12-27 23:25:42",
  "elapsed_time": 210,
  "organization_id": 1,
  "availability_status": {
    "kind": "away",
    "name": "Con Cliente"
  },
  "person": {
    "id": 2,
    "type": "User",
    "email": "pedropablo@gmail.com"
  }
  "network_status": {
    "id": 2,
    "online": 1,
    "timestamp": 1552421559
  }
}
```
*Las fechas están en hora local

<!-- tabs:end -->

<!-- div:left-panel -->

Obtención  histórica de los estados personalizados de los ***usuario/agente*** filtrado por fecha

### HTTP Request

`GET https://api.letsta.lk/api/v1/custom_status_histories`

!>Este <i>endpoint</i> solo puede ser utilizado por el tipo <strong>usuario/agente</strong>.

### Query Parameters

| Parámetro  | ¿Requerido? | Descripción                                                                  |
|------------|-------------|------------------------------------------------------------------------------|
| auth_token | si          | Token de autenticación                                                       |
| from       | si          | Fecha y hora de inicio de los datos  a consultar. Formato `AAAA-MM-DD HH:MM` |
| to         | si          | Fecha y hora de fin de los  datos  a consultar. Formato `AAAA-MM-DD HH:MM`   |
| page       | no          | Número de página a obtener de los datos

### Body Parameters

Sin parámetros.

### Códigos de respuesta

| Código | Significado                    |
|--------|--------------------------------|
| 401    | Credenciales inválidas.        |
| 200    | Operación ejecutada con éxito. |

<!-- panels:end -->
