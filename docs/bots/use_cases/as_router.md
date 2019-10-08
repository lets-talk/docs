# BOT como router

## Crear conversación en Let's Talk y enviar mensajes entre agente y cliente WhatsApp


1. BOT mantiene conversación con cliente de Whatsapp y decide derivar a un agente humano en Let's Talk
2. BOT se suscribe a PubNub
3. BOT crea un cliente en Let's Talk utilizando el endpoint de la API de Let's Talk [Crear/obtener un cliente](https://apidoc.ltmessenger.com/#crear-obtener-un-cliente)
4. BOT memoriza el token del cliente que retornal el paso anterior
5.BOT inicia una conversación en Let's Talk utilizando el endpoint de la API de Let's Talk [Crear conversación](https://apidoc.ltmessenger.com/#crear-conversacion).
  1. En el campo mensaje concatena todos los mensajes anteriores de la conversación de whatsapp para que el ***usuario/agente*** lea el contexto de la conversación.

