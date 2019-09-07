# Inicialización por parámetros de URL

Es posible iniciar un chat a través de un link que levantará automáticamente un widget web sin necesidad de instalar el código javascript en su sitio. 

## Uso simple

Para utilizar la forma simple se debe solicitar a Let's Talk, vía soporte@ltmessenger.com, un ***string*** en forma de cadena ***Base64*** que será necesario pasar como parámetro `preset` en el [***Query string***](https://en.wikipedia.org/wiki/Query_string) de una ***URL***

El enlace para levantar un chat tiene la siguiente forma:

`https://[subdomain].letsta.lk/api/v1/preset_chat?[query_params]`

Donde la variable `[subdomain]` corresponde al subdominio de la organización.

### Query Params

#### Query params requeridos

La especificación de los `query_params` requeridos es la siguiente:

- `preset`: 
  - Tipo: `string`
  - Requerido: **Si**
  - Descripción: Cadena base64 proveída por Let's Talk
- `visitor_name`: 
  - Tipo: `string`
  - Requerido: **Si**
  - Descripción: Nombre del cliente/***visitor*** que iniciará el chat.
- `visitor_email`: 
  - Tipo: `string`
  - Requerido: **Si**
  - Descripción: Email del cliente/***visitor*** que iniciará el chat.

A continuación se ofrece un ejemplo de la ***URL***:

`https://bancoamericano.letsta.lk/api/v1/preset_chat?preset=c2V0dGluZ3M&visitor_name=Juan&visitor_email=juan@letsta.lk`

##### URL encoding

Si el nombre o valor de los parámetros contienen caracteres no seguros para un ***URL***, entonces la ***URL*** deberá enviarse según el [Código porciento](https://es.wikipedia.org/wiki/C%C3%B3digo_porciento).

Una herramienta en línea muy útil para transformar una ***URL*** con caracteres no seguros puede usarse en https://www.url-encode-decode.com/

Por ejemplo, si necesitamos utilizar el siguiente enlace con parámetros no seguros:

`https://bancoamericano.letsta.lk/api/v1/preset_chat?preset=c2V0dGluZ3M&visitor_name=Juan Pérez Güilla&visitor_email=juan@letsta.lk`

La ***URL*** transformada según la herramienta descrita anteriormente sería la siguiente:

`https://bancoamericano.letsta.lk/api/v1/preset_chat?preset%3Dc2V0dGluZ3M%26visitor_name%3DJuan+P%C3%A9rez+G%C3%BCilla%26visitor_email%3Djuan%40letsta.lk`


#### Metadatos

Es posible utilizar parámetros personalizados opcionales para agregar metadatos tanto al cliente como al chat. Pueden ser usados tantos parámetros mientras el largo total de la ***URL*** no sobrepase los 2000 caracteres. Más información en [este hilo de stack overflow](https://stackoverflow.com/questions/417142/what-is-the-maximum-length-of-a-url-in-different-browsers).

#### Metadatos del cliente

Los ***query params*** deben tener el siguiente formato: 

`visitor_attrs_[nombre_parametro]=[valor_parametro]`

Dónde:

- `nombre_parametro `: 
  - Tipo: `string`
  - Requerido: **Si**
  - Descripción: Nombre de parámetro personalizado
- `valor_parametro `: 
  - Tipo: `string` o `integer`
  - Requerido: **Si**
  - Descripción: Valor del parámetro personalizado

Ejemplo:

Si deseamos enviar atributos del cliente como edad, sexo y estado civil, los ***query params*** serían los siguiente:

`visitor_attrs_edad=45&visitor_attrs_sexo=masculino&visitor_attrs_estado_civil=soltero`

Si los ***query params*** contienen caracteres no seguros, aplicar el método descrito en [URL encoding](#url-encoding)

#### Metadatos del chat

Los ***query params*** deben tener el siguiente formato: 

`chat_metadata_[nombre_parametro]=[valor_parametro]`

Dónde:

- `nombre_parametro `: 
  - Tipo: `string`
  - Requerido: **Si**
  - Descripción: Nombre de parámetro personalizado
- `valor_parametro `: 
  - Tipo: `string` o `integer`
  - Requerido: **Si**
  - Descripción: Valor del parámetro personalizado

Ejemplo:

Si deseamos enviar atributos del chat como fuente, canal y prioridad, los ***query params*** serían los siguiente:

`chat_metadata_fuente=email&chat_metadata_canal=postventa&chat_metadata_prioridad=10`

Si los ***query params*** contienen caracteres no seguros, aplicar el método descrito en [URL encoding](#url-encoding)

### Ejemplo completo

La url completa unificando los ejemplos anteriores sería la siguiente:

`https://bancoamericano.letsta.lk/api/v1/preset_chat?preset=c2V0dGluZ3M&visitor_name=Juan&visitor_email=juan@letsta.lk&visitor_attrs_edad=45&visitor_attrs_sexo=masculino&visitor_attrs_estado_civil=soltero&chat_metadata_fuente=email&chat_metadata_canal=postventa&chat_metadata_prioridad=10`
