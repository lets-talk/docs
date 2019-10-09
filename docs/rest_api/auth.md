# Autenticación

<!-- panels:start -->

<!-- div:right-panel -->

?> Para autorizar una **request** puedes utilizar el siguiente código:

<!-- tabs:start -->
#### ** Request **

```shell
curl "api_endpoint_here"
  -H "Authorization: Basic $(echo -n 'raw_api_key:X' | base64)"
```
<!-- tabs:end -->

?> Asegúrate de reemplazar `raw_api_key` con tu *API Key*.

<!-- div:left-panel -->

Let's Talk utiliza *API Keys* para la autenticación. La *API* puede ser usada como  **usuario/cliente** o  **usuario/agente**

- **usuario/agente**: Usuario que se encarga de atender las conversaciones por el lado de la organización.
- **usuario/cliente**: Usuario que inicia una conversación desde algunos de los canales disponibles como el *widget* web o la aplicación móvil.

La formas de obtener una *API Key* depende del tipo de usuario que interectuará con la API:

- **usuario/agente**: Solicitar una *API Key* al correo [soporte@ltmessenger.com](mailto:soporte@ltmessenger.com)
- **usuario/cliente**: Solicitar un `consumer_key`, `consumer_token` y `organization_id` al correo [soporte@ltmessenger.com](mailto:soporte@ltmessenger.com) para utilizar el endpoint [Crear/obtener un cliente](#Crear/obtener un cliente) y obtener una API Key de **usuario/cliente**.

La *API Key* debe ser incluida en todas las peticiones a los *endpoints* autenticados como *header* según el protocolo [Basic access authentication](https://en.wikipedia.org/wiki/Basic_access_authentication).

Cuando recibas la *API Key* es necesario codificarla en `base64` adicionando los caractéres `:X`.

El *header* se ve como lo siguiente:

`Authorization: encoded_api_key== `

!> Debes reemplazar <code>encoded_api_key== </code> por tu <i>API Key</i>, con los caracteres <code>`:X`</code> concatenados al final y codificada en `base64`

[filename](../rest_api/_response_codes.md ':include')


<!-- panels:end -->
