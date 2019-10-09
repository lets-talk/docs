# Grupo de Conversaciones
## Asignar grupo a conversación.

<!-- panels:start -->

<!-- div:right-panel -->

<!-- tabs:start -->
#### ** Request **

```shell
curl -X POST \
  https://api.letsta.lk/api/v1/conversations/{conversation_id}/conversation_groups \
  -H 'Accept: application/json, text/plain, */*' \
  -H "Authorization: Basic {{encoded_agent_api_key}}" \
  -H 'Content-Type: application/json;charset=UTF-8' \
  -d '{"conversationId":"{conversation_id}","{group_id}":{group_id},"transfer":false}'
```
<!-- tabs:end -->

?> El comando previo retorna un JSON como el siguiente:

<!-- tabs:start -->
#### ** Response **

```json
{
    "id": 1,
    "group_id": 2,
    "conversation_id": 827,
    "created_at": "2014-12-23T18:53:14.000+0000",
    "group": {
        "id": 2,
        "name": "ventas",
        "auto_join": false
    }
}
```
<!-- tabs:end -->

<!-- div:left-panel -->

Al asignar un grupo a una conversación es opcional que el usuario que realiza la acción abandone la conversación, para esto el parámetro `transfer` debe ir `true`.
Tener en consideración que al asignar a un grupo la forma en que los miembros del dicho grupo de unen a la conversación depende de sus propias reglas y no de cómo se le asigne.
Un mensaje tipo `SYSTEM` es agregado indicando quién realizó la acción de asignación.


!> Este <i>endpoint</i> solo puede ser utilizado por el tipo <strong>usuario/agente</strong>

### HTTP Request

`POST https://api.letsta.lk/api/v1/conversations/{conversation_id}/conversation_groups`

### Body Parameters

| Propiedad       | Tipo    | ¿Requerido? | Descripción                                                                                        |
|-----------------|---------|-------------|----------------------------------------------------------------------------------------------------|
| group_id        | integer | si          | Id del grupo que recibirá la conversación. Este Id debe solicitarse a soporte para cada caso       |
| conversation_id | integer | si          | Id del inquiry de la conversación                                                                  |
| transfer        | bool    | no          | Al indicar `true` el sistema eliminará la memebresía actual del usuario que realiza la asignación. |



[filename](../rest_api/_response_codes.md ':include')

<!-- panels:end -->
