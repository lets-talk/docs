# BOT como agente

## Atención exclusiva de BOT


1. BOT se suscribe a PubNub
2. **_msgPN_** con `namespace conversation_memberships.create `o` namespace message.create `es recibido por el bot
3. BOT guarda `conversation_id` en una lista de conversaciones activas
4. BOT hace llamada al endpoint [Obtener mensajes de conversación](https://apidoc.ltmessenger.com/#obtener-mensajes-de-conversacion) para obtener mensajes anteriores de la conversación, si los hubiera
5. BOT hace llamada al endpoint _Obtener información de un cliente_ para obtener perfil del cliente
6. BOT se mantiene escuchando **_msgPN_** con `namespace message.create` y parámetros descritos en punto [Evento de creación de mensaje de cliente](bots/pubnub?id=evento-de-creación-de-mensaje)
    1. BOT envía mensaje de respuesta mediante el endpoint [Enviar mensaje](https://apidoc.ltmessenger.com/#crear-mensaje-de-texto-plano)
7. Una vez terminada la conversación, el BOT cierra la conversación mediante endpoint [Cerrar conversación](https://apidoc.ltmessenger.com/#cerrar-conversacion)


## Atención exclusiva de BOT y transferencia a grupo de ejecutivos



1. BOT se suscribe a PubNub
2. _msgPN_ con `namespace conversation_memberships.create `o` namespace message.create `es recibido por el bot
3. BOT guarda `conversation_id` en una lista de conversaciones activas
4. BOT hace llamada al endpoint [Obtener mensajes de conversación](https://apidoc.ltmessenger.com/#obtener-mensajes-de-conversacion) para obtener mensajes anteriores de la conversación, si los hubiera
5. BOT hace llamada al endpoint _Obtener información de un cliente_ para obtener perfil del cliente
6. BOT se mantiene escuchando **_msgPN_** con `namespace message.create` y parámetros descritos en punto [Evento de creación de mensaje de cliente](bots/pubnub?id=evento-de-creación-de-mensaje)
    1. BOT envía mensaje de respuesta mediante el endpoint [Enviar mensaje](https://apidoc.ltmessenger.com/#crear-mensaje-de-texto-plano)
7. BOT es incapaz de continuar la conversación y necesita transferirla a un grupo de ejecutivos
    1. BOT transfiere la conversación mediante endpoint [Transferir conversación a un Grupo](https://apidoc.ltmessenger.com/#asignar-grupo-a-conversacion)
    2. Bot abandona la conversación mediante endpoint _Eliminar usuario de una conversación_


## Asistencia al ejecutivo



1. BOT se suscribe a PubNub
2. _msgPN_ con `namespace conversation_memberships.create `o` namespace message.create `es recibido por el bot
3. BOT guarda `conversation_id` en una lista de conversaciones activas
4. BOT hace llamada al endpoint [Obtener mensajes de conversación](https://apidoc.ltmessenger.com/#obtener-mensajes-de-conversacion) para obtener mensajes anteriores de la conversación, si los hubiera
5. BOT hace llamada al endpoint _Obtener información de un cliente_ para obtener perfil del cliente
6. BOT se mantiene escuchando **_msgPN_** con `namespace message.create` y parámetros descritos en punto [Evento de creación de mensaje de cliente](bots/pubnub?id=evento-de-creación-de-mensaje)
    1. BOT envía mensaje interno de respuesta mediante el endpoint [Enviar mensaje](https://apidoc.ltmessenger.com/#crear-mensaje-de-texto-plano) de acuerdo lógica de recomendación interna del bot


## Escalamiento automático según tono de la conversación



1. BOT se suscribe a PubNub
2. _msgPN_ con `namespace conversation_memberships.create `o` namespace message.create `es recibido por el bot
3. BOT guarda `conversation_id` en una lista de conversaciones activas
4. BOT hace llamada al endpoint [Obtener mensajes de conversación](https://apidoc.ltmessenger.com/#obtener-mensajes-de-conversacion) para obtener mensajes anteriores de la conversación, si los hubiera
5. BOT hace llamada al endpoint _Obtener información de un cliente_ para obtener perfil del cliente
6. BOT se mantiene escuchando **_msgPN_** con `namespace message.create` y parámetros descritos en punto [Evento de creación de mensaje de cliente](bots/pubnub?id=evento-de-creación-de-mensaje)
    1. Si BOT determina el que cliente está MUY ENOJADO, entonces:
        1. Envía mensaje para el cliente de que a conversaciónserá escalada mediante el endpoint [Enviar mensaje](https://apidoc.ltmessenger.com/#crear-mensaje-de-texto-plano)
        2. BOT elimina a agente de la conversación mediante endpoint _Eliminar usuario de una conversación_
        3. BOT transfiere conversación a grupo de supervisores mediante endpoint [Transferir conversación a un grupo](https://apidoc.ltmessenger.com/#asignar-grupo-a-conversacion)
