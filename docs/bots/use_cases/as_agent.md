# BOT como agente

## Atención exclusiva de BOT


1. BOT se suscribe a PubNub
2. **_msgPN_** con `namespace conversation_memberships.create `o` namespace message.create `es recibido por el bot
3. BOT guarda `conversation_id` en una lista de conversaciones activas
4. BOT hace llamada al endpoint [Obtener mensajes de conversación](/rest_api/messages?id=obtener-mensajes-de-conversación) para obtener mensajes anteriores de la conversación, si los hubiera
5. BOT hace llamada al endpoint [Obtener información de un cliente](/rest_api/clients?id=obtener-información-de-un-cliente) para obtener perfil del cliente
6. BOT se mantiene escuchando **_msgPN_** con `namespace message.create` y parámetros descritos en punto [Evento de creación de mensaje de cliente](bots/pubnub?id=evento-de-creación-de-mensaje)
    1. BOT envía mensaje de respuesta mediante el endpoint [Enviar mensaje](/rest_api/messages?id=crear-mensaje-de-texto-plano)
7. Una vez terminada la conversación, el BOT cierra la conversación mediante endpoint [Cerrar conversación](/rest_api/conversations?id=cerrar-conversación)


## Atención exclusiva de BOT y transferencia a grupo de ejecutivos



1. BOT se suscribe a PubNub
2. _msgPN_ con `namespace conversation_memberships.create `o` namespace message.create `es recibido por el bot
3. BOT guarda `conversation_id` en una lista de conversaciones activas
4. BOT hace llamada al endpoint [Obtener mensajes de conversación](/rest_api/messages?id=obtener-mensajes-de-conversación) para obtener mensajes anteriores de la conversación, si los hubiera
5. BOT hace llamada al endpoint [Obtener información de un cliente](/rest_api/clients?id=obtener-información-de-un-cliente) para obtener perfil del cliente
6. BOT se mantiene escuchando **_msgPN_** con `namespace message.create` y parámetros descritos en punto [Evento de creación de mensaje de cliente](bots/pubnub?id=evento-de-creación-de-mensaje)
    1. BOT envía mensaje de respuesta mediante el endpoint [Enviar mensaje](/rest_api/messages?id=crear-mensaje-de-texto-plano)
7. BOT es incapaz de continuar la conversación y necesita transferirla a un grupo de ejecutivos
    1. BOT transfiere la conversación mediante endpoint [Transferir conversación a un Grupo](/rest_api/conversation_groups?id=asignar-grupo-a-conversación)


## Asistencia al ejecutivo



1. BOT se suscribe a PubNub
2. _msgPN_ con `namespace conversation_memberships.create `o` namespace message.create `es recibido por el bot
3. BOT guarda `conversation_id` en una lista de conversaciones activas
4. BOT hace llamada al endpoint [Obtener mensajes de conversación](/rest_api/messages?id=obtener-mensajes-de-conversación) para obtener mensajes anteriores de la conversación, si los hubiera
5. BOT hace llamada al endpoint [Obtener información de un cliente](/rest_api/clients?id=obtener-información-de-un-cliente) para obtener perfil del cliente
6. BOT se mantiene escuchando **_msgPN_** con `namespace message.create` y parámetros descritos en punto [Evento de creación de mensaje de cliente](bots/pubnub?id=evento-de-creación-de-mensaje)
    1. BOT envía mensaje interno de respuesta mediante el endpoint [Enviar mensaje](/rest_api/messages?id=crear-mensaje-de-texto-plano) de acuerdo lógica de recomendación interna del bot


## Escalamiento automático según tono de la conversación



1. BOT se suscribe a PubNub
2. _msgPN_ con `namespace conversation_memberships.create `o` namespace message.create `es recibido por el bot
3. BOT guarda `conversation_id` en una lista de conversaciones activas
4. BOT hace llamada al endpoint [Obtener mensajes de conversación](/rest_api/messages?id=obtener-mensajes-de-conversación) para obtener mensajes anteriores de la conversación, si los hubiera
5. BOT hace llamada al endpoint [Obtener información de un cliente](/rest_api/clients?id=obtener-información-de-un-cliente) para obtener perfil del cliente
6. BOT se mantiene escuchando **_msgPN_** con `namespace message.create` y parámetros descritos en punto [Evento de creación de mensaje de cliente](bots/pubnub?id=evento-de-creación-de-mensaje)
    1. Si BOT determina el que cliente está MUY ENOJADO, entonces:
        1. Envía mensaje para el cliente de que a conversaciónserá escalada mediante el endpoint [Enviar mensaje](/rest_api/messages?id=crear-mensaje-de-texto-plano)
        2. BOT transfiere conversación a grupo de supervisores mediante endpoint [Transferir conversación a un grupo](/rest_api/conversation_groups?id=asignar-grupo-a-conversación)
