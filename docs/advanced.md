# Instalación Avanzada Web Chat Let’s Talk

Además de la opción básica, el chat web acepta una serie de parámetros adicionales para la customización gráfica, pero también para la operación.

## Variables de Instalación
En las distintas fases y versiones de instalación de hace referencia a variables entre paréntesis cuadrados `[]`, esto quiere decir que se deben usar variables propias de la instancia a implementar. En caso de no contar con estas variables comunicarse con la/el encargada/o del proyecto o con soporte@ltmessenger.com.

```
[organization_subdomain]: YOUR_ORGANIZATION_NAME
[widget_name]: YOUR_WIDGET_NAME
[consumer_key]: xxxxxxxxxxxxxxxxx
[consumer_token]: zzzzzzzzzzzzzzzz
```

## Sesión de usuario (Utilizacion en sitio privado)

### Autenticación Simple

En particular, para sitios que posean una **sesión con usuario identificado**, es decir, donde ya se sabe quién es el visitante el widget permite ser inicializado con una sesión activa para el visitante autorizada por las credenciales de la misma organización. Esta funcionalidad es útil cuando se usa el widget en sitios privados como: intranet, web internas con usuarios con sesión iniciada.

A cada organización se le provee una pareja **(`consumer_key`, `consumer_token`)** de credenciales que permite integrar su propia logica de authenticación de usuarios. Si no posee esta información deberá solicitarlo escribiendo a **soporte@letstalk.com**.
Deberá proveer dicha pareja al momento de la inicialización del chat.

Al inicializar el widget con **consumer** setting y **visitor** **el visitante no tendrá que iniciar sesión** en el widget.

*Ejemplo*:

```javascript
window.$LT(function(messenger) {
  messenger.setByName("[widget_name]");
  messenger.settings({
  	eager_loading: true,
  	consumer: {
    	  key:   '[consumer_key]',
    	  token: '[consumer_token]'
  	},
  	visitor: {
    	  name:  'Client Demo',
    	  email: 'client.demo@letsta.lk',
      	attrs: {
          sucursal: 'Sucursal Providencia',
          tipoBanca: 'Banca Personal',
          convenio: 'Convenio Pyme',
      	}
  	}
  });
});
```

### Autenticación Avanzada

Para sitio que requieran un capa adicional de seguridad es posible establecer un autenticación vía `HMAC 256` y un secreto compartido. El flujo de la autenticación es el siguiente:

1. Adicionalmente a la pareja **(`consumer_key`, `consumer_token`)**, se  debe solicitar a Let's Talk la generación de un `secreto`  compartido escribiendo a  **soporte@letstalk.com**. Este secreto será utilizado para el computo de un hash `HMAC 256`.
2. El `secreto` debe mantenerse privado y nunca divulgarse.
3. Cada vez que se inicialice el widget, el cliente, vía **lenguaje de servidor**, debe computar un hash usando la función `HMAC 256` con el `secreto` y el `uid` del visitante. Ejemplos de código para diversos lenguajes pueden ser vistos en el siguiente enlace: https://www.jokecamp.com/blog/examples-of-creating-base64-hashes-using-hmac-sha256-in-different-languages/. 
En el caso de ruby sería de la siguiente forma:
```ruby
OpenSSL::HMAC.hexdigest(
  'sha256', # funcion de hash
  'secreto-de-organizacion', # llave secreta (¡mantener segura!)
  current_user.email #  uid del cliente
)
```
4. El hash generado debe enviarse como parámetro `client_hash` en la inicialización del widget:

```javascript
window.$LT(function(messenger) {
  messenger.setByName("[widget_name]");
  messenger.settings({
  	eager_loading: true,
  	consumer: {
    	  key:   '[consumer_key]',
    	  token: '[consumer_token]'
  	},
    client_hash: '[hash_computado]'
  	visitor: {
    	  name:  'Client Demo',
    	  email: 'client.demo@letsta.lk',
      	attrs: {
          sucursal: 'Sucursal Providencia',
          tipoBanca: 'Banca Personal',
          convenio: 'Convenio Pyme',
      	}
  	}
  });
});
```
4. Es muy importante que el `client_hash` usando el `secreto` se genere en el *lado del servidor* y nunca en el lado del cliente. Si el `secreto` se publicara en el lado del cliente la seguridad de la autenticación se vería comprometida.
6. El backend de Let's Talk, utilizando el `secreto`y el `email` computa la función `HMAC 256` y obtiene un hash. Si el hash computado es igual al `client_hash` recibido entonces autoriza la request. En caso contrario la rechaza.

## Atributos del usuario

Además de poder iniciar el widget con un usuario logueado podrá agregar información al perfil del usuario pasando datos extra en el objeto **visitor.attrs**. Esta información puede ser muy valiosa para el agente que reciba la conversación ya que le brinda mayor contexto sobre con quien va a interactuar.

Para esto se debe entregar un objeto **visitor** que sólo será aceptado si viene con las credenciales correctas del consumer (la organización) como se vió en el ejemplo anterior.
Los valores para **name** y **email** son requeridos en el objecto visitor, mientras que attrs puede omitirse.

El uso esperado es que la web de la organización complete el objeto visitor con los datos necesarios antes de inicializar el widget.

## Atributos del contexto

En la función **chatMetadata** se pueden adicionar parámetros personalizados que se mostrarán como un mensaje interno al inicio de la conversación. El objetivo es brindar información relevante al agente que atenderá al cliente.

*Ejemplo:*

```javascript
messenger.chatMetadata(function() {
  return {
    empresa: window.location.href.match(/pyme/) ? 'pyme': 'no-pyme',
  }
});
```
En el ejemplo se envía el atributo empresa con el valor pyme o no-pyme dependiendo de la url de carga del widget, lo que indicará al agente si se inició la conversación en el portal pyme o no-pyme.

## Filtros de inicialización

En ocasiones existen en el contexto de inicialización variables que discriminan si el widget debe o no cargarse, es decir, que para ciertos valores dinámicos de la web la inicialización se realiza y para el resto no.

Un caso clásico es alguna marca en la sesión, por ejemplo el `tipoCliente`, supongamos que una organización quiere mostrar el chat sólo a los clientes que tienen sesión con `tipoCliente: 'recurrente'`, entonces en este caso si el `tipoCliente` no corresponde al valor _recurrente_ el chat no se despliega. Para lograr esto sin obligar a poner lógicas estáticas en la web de la organización ofrecemos los filtros de carga.

Los filtros consisten en listas de valores arbitrarios (_strings_) que se definen en la plataforma, luego el chat envía un valor de conrexto  y si dicho valor se encuentra dentro fuera de la lista definida entonces se despliega o bloquea el chat.

Los filtros disponibles son 2: `black_list_value` de bloqueo y `white_list_value` de permiso. 
Cuando el chat envía un `black_list_value` el sistema verifica que el valor **NO** se encuentre en la lista de valores bloqueados para desplegarse. Cuando el chat envía un `white_list_value` el sistema verifica que el valor **SI** se encuentre en la lista de valores permitidos para desplegarse.
El siguiente es un ejemplo donde el chat sólo se despliega si es que _recurrente_ se encuentra en la lista definida en el backend.

```javascript
window.$LT(function(messenger){
        messenger.setByName("widget-1");
        messenger.settings({
            white_list_value: 'recurrente',
            // black_list_value: 'bloqueado',
            consumer: {
                key: 'consumer_key',
                token: 'consuumer_token'
            },
            visitor: {
                name: name,
                email: email,
                attrs: {
                    proyecto: 'piloto'
                }
            }
        });
    });
```

Por consistencia el uso de los filtros es excluyente en la inicialización del chat (`black_list_value` o `white_list_value`)
Para definir una lista de filtros deben solicitar su configuración a soporte@ltmessenger.com.
