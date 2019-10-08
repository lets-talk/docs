# Pubnub

## Contexto

Para que el bot pueda escuchar las conversaciones y sus mensajes, es necesario que se suscriba a dos canales de nuestro proveedor de Realtime IaaS, llamado PubNub, e interactuar con nuestra API.

Cuándo un usuario es creado, ya sea agente o cliente, automáticamente se le asigna un canal de PubNub llamado `user-[user-id]-channel` para ***usuario/agente***,  y `client-[user-id]-channel` para ***usuario/cliente***, donde escuchará diversos tipos de mensajes (en adelante ***msgPN***), entre ellos:

* Mensajes de organización
* Mensajes de una conversación
* Mensajes de membresías de conversaciones

Además es necesario suscribirse a otro canal llamado `presence.user-[user-id]` o `presence.client-[user-id]`, que al conectarse, indicará a Let’s Talk automáticamente eventos de online/offline para mantener el estado de la conexión del usuario.


En general un msgPN tiene el siguiente payload.

```json
{
  "channel": "conversation-686949",
  "subscription": "user-5207-channel",
  "actualChannel": "conversation-686949",
  "subscribedChannel": "user-5207-channel",
  "timetoken": "15356577183544050",
  "publisher": "a5fd7870-169d-492a-aa08-7dde77eb331f",
  "message": {
    "namespace": "conversation_memberships.create",
    "data": {
      ...
    }
  }
}
```

Cada uno de esos tipos de msgPN se discrimina a través del parámetro namespace. En el objeto data vienen distintos datos dependiendo del namespace.

## Tipos de msgPN

Los msgPN relevantes para la integración de un bot son que tienen ***namespace*** `conversation_memberships.create` y `messages.create`. A continuación se muestran ejemplos de ***payload*** y luego su explicación.

### Evento de asignación de conversación a un usuario/agente

#### Payload

```json
{
   "namespace":"conversation_memberships.create",
   "data":{
      ...
      "person_id":5207,
      "person_type":"User",
      "conversation_id":685218,
      "role":"Member",
      ...
      "conversation_status":"Open",
      ...
      "conversation":{
         "id":685218,
         "issue":"Hola",
         "internal_issue":"Hola",
         ...
         "status":"Open",
         "client":{
            "id":5820753,
            "name":"Juanito Pérez",
            "email":"kanupopas@mailinator.net"
            ...
         }
         ...
      }
      ...
   }
   ...
}
```

#### Descripción

Este msgPN se gatilla cuando un ***usuario/agente*** se une a una conversación. En el objeto `data` viene información respecto al tipo de membresía (`role`), id de la conversación (`conversation_id`) e id de la persona (`person_id`). Si `person_id` es igual nuestro [user-id] entonces la conversación fue asignada a nuestro ***usuario/agente***. Dependiendo del tipo de membresía, pueden darse dos casos:

- `"role": "Member"` : Nuestro ***usuario/agente*** fue asignado a la conversación como miembro. Es su responsabilidad interactuar directamente con el cliente.
- `"role": "Viewer"` : Nuestro ***usuario/agente*** fue asignado a la conversación como viewer. Puede recibir los mensajes de la conversación, pero no tiene la responsabilidad de interactuar con el cliente.

##### Consideración 1
!> Para identificar los mensajes futuros de una conversación es necesario guardar el `conversation_id`, valor que será usado para filtrar los ***msgPN*** del ***namespace*** `messages.create` (ver punto [Evento de creación de mensaje](#evento-de-creación-de-mensaje)).

##### Consideración 2
!> Cada vez que se reciba este ***msgPN*** es necesario hacer una llamada a la API Let’s Talk para obtener todos los mensajes anteriores de la conversación si los hubiera, ya que la asignación al ***usuario/agente*** podría hacerse en cualquier momento de la conversación. La descripción de este endpoint puede verse en el punto Obtener mensajes de conversación.

##### Consideración 3
!>Puede que este evento nunca sea recibido, y solo se reciban los eventos de creación de mensaje de cliente. Si este fuera el caso, entonces deben aplicarse las mismas consideraciones anteriores: [Consideración 1](#consideración-1) y [Consideración 2](#consideración-2) para el primer mensaje de una nueva conversación.


### Evento de creación de mensaje

#### Payload

```json
{
  "namespace": "messages.create",
  "data": {
    "id": 13130611,
    "subject": null,
    "content": "Hola",
    "type": "NORMAL",
    ...
    "conversation_id": 685218,
    ...
    "conversation_type_name": "normal",
    "conversation_internal_issue": "Hola",
    "client_name": "Juanito Pérez",
    ...
    "conversation_status": "Open",
    "person_type": "Client",
    "person_id": 5820753,
    ...
    }
  }
}
```

#### Descripción

Este ***msgPN*** se gatilla cuando se recibe un mensaje de una conversación.

Para identificar los mensajes de una conversación de un ***usuario/cliente*** se deben utilizar el siguiente filtro sobre los eventos:

| Propiedad       | Valor                            |
|-----------------|----------------------------------|
| conversation_id | Identificador de la conversación |
| person_type     | `Client`                         |
| person_id       | Id del cliente                    |
| type            | `NORMAL`                         |

Para identificar los mensajes de una conversación de un ***usuario/agente*** se deben utilizar el siguiente filtro sobre los eventos:

| Propiedad       | Valor                            |
|-----------------|----------------------------------|
| conversation_id | Identificador de la conversación |
| person_type     | `User`                           |
| person_id       | Id del usuario                   |
| type            | `NORMAL`                         |


Si se da el caso en que el mensaje corresponde a una nueva conversación (por ser un `conversation_id` nuevo), es necesario tener en cuenta lo siguiente

##### Consideración 1
!>Para identificar los mensajes futuros de una conversación se debe guardar el `conversation_id`, valor que será usado para filtrar los siguientes mensajes para identificarlos como pertenecientes a una conversación.

##### Consideración 2
!>Se debe hacer una llamada a la [API de Let’s Talk](https://apidoc.ltmessenger.com) para obtener todos los mensajes anteriores de la conversación si los hubiera, ya que la asignación al ***usuario/agente*** podría hacerse en cualquier momento de la conversación y no necesariamente el mensaje que llega es el primer mensaje. La descripción de este endpoint puede verse en el endpoing [Obtener mensajes de conversación](https://apidoc.ltmessenger.com/#obtener-mensajes-de-conversacion)

## Conexión a Pubnub

PubNub provee más de 70 SDKs para distintos tipos de lenguajes. La documentación al respecto puede ser consultada en el siguiente enlace:

https://www.pubnub.com/docs

Los siguientes ejemplos son implementados en lenguaje JavaScript, SDK versión 4. La documentación puede ser visualiza en el siguiente enlace:

 https://www.pubnub.com/docs/javascript/pubnub-javascript-sdk-v4.


### Ejemplo HTML + Javascript

A continuación se presenta un archivo HTML de ejemplo que implementa la suscripción a PubNub utilizando el SDK de JavaScript.

```HTML
<html>
<head>
 <script src="https://cdn.pubnub.com/sdk/javascript/pubnub.4.21.5.js"></script>
</head>
<body>
 <h1>Pubnub</h1>
<script>
  data = {
    user_id: [user-id],
    auth_key: '[auth_key]',
    subkey: '[subkey]',
    pubkey: '[pubkey]'
  }

  var uuid = "user-" + data.user_id;
  var userChannel = 'user-' + data.user_id + '-channel';
  var userPresence = "presence." + uuid;

  var pubnub = new PubNub({
        publishKey   : data.pubkey,
        subscribeKey : data.subkey,
        uuid: uuid,
        authKey: data.auth_key,
        ssl: true,
        restore: true
      });

  pubnub.addListener({
      message: function(m) {
          payload = m.message
          msgPN = {
            namespace: payload.namespace,
            data: payload.data
          }
   // Se filtran algunos namespaces que no interesan para el ejemplo
          if (msgPN.namespace.match(/person_statuses|notifications|typing|custom_rules/) == null){
            console.log(msgPN)
          }
      }
  });

  pubnub.subscribe({
    channels: [userPresence, userChannel]
  });
 </script>
</body>
</html>

```
