# Reportes
## Estados personalizados históricos

<!-- panels:start -->

<!-- div:right-panel -->

<!-- tabs:start -->
#### ** Request **

```shell
curl --location --request GET "https://api.letsta.lk/api/v1/reports/custom_status_histories?from=2020-01-28%2012:26:00&to=2020-01-28%2012:27:00&page=1&per_page=2" \
--header "Authorization: Basic Q0V6cHJ0M2R6ckdHeGV6eHpob2Q6WA==" \
--header "Content-Type: application/json"
```
<!-- tabs:end -->

?> El comando previo retorna un JSON como el siguiente:

<!-- tabs:start -->
#### ** Response **
```json
{
  "success": true,
  "pagination": {
    "total_records": 1,
    "total_pages": 1,
    "per_page": 2,
    "page": 1
  },
  "custom_status_histories": [
    {
      "id": 1,
      "start_date": "2018-01-01 07:00:00",
      "end_date": "2018-01-01 08:00:00",
      "elapsed_time": 3600,
      "organization_id": 2,
      "availability_status": {
        "kind": "active",
        "name": "Trabajando"
      },
      "person": {
        "id": 5,
        "type": "User",
        "email": "1beatriz@mesa.es"
      }
    }
  ]
}
```
*Las fechas están en hora local según la configuración de su organización

<!-- tabs:end -->

<!-- div:left-panel -->

Obtención  histórica de los estados personalizados de los ***usuario/agente*** filtrado por fecha

### HTTP Request

`GET https://api.letsta.lk/api/v1/reports/custom_status_histories`

!>Este <i>endpoint</i> solo puede ser utilizado por el tipo <strong>usuario/agente</strong>.

### Query Parameters

| Parámetro  | ¿Requerido? | Descripción                                                                     |
|------------|-------------|---------------------------------------------------------------------------------|
| auth_token | si          | Token de autenticación                                                          |
| from       | si          | Fecha y hora de inicio de los datos  a consultar. Formato `AAAA-MM-DD HH:MM:SS` |
| to         | si          | Fecha y hora de fin de los  datos  a consultar. Formato `AAAA-MM-DD HH:MM:SS`   |
| page       | no          | Número de página a obtener de los datos
| per_page   | no          | Número de elementos por página. Máximo 25.

### Body Parameters

Sin parámetros.

### Códigos de respuesta

| Código | Significado                    |
|--------|--------------------------------|
| 400    | Parámetros inválidos.       |
| 401    | Credenciales inválidas.        |
| 200    | Operación ejecutada con éxito. |

<!-- panels:end -->
