# Chat Api Metodos Publicos

!> **Importante** La app de Chat tiene 2 métodos principales: `on` y `executeMethod`.


## on
#### on (eventName: string, callback: Function): Promise

Este método permite subscribir una función `callback` para que se ejecute cuando el evento del chat `eventName` ocurre.


```javascript
  chatApp.on('[EVENT_NAME]', function() {

  });
```

## executeMethod
#### executeMethod (method: string, paramsObject: Object): Promise

Este método permite ejecutar una función `method` disponible por el Chat con los parámetros deseados.

```javascript
  chatApp.executeMethod('[METHOD_NAME]', paramsObject)
    .then(() => {
      // Exito al ejecutar el método
    })
    .catch((err) => {
      // Ocurrió un error
    });
```

## *Metodos disponibles*
### *setDisplayState*

Permite cambiar el estado de la caja de Chat.

#### params

```javacript
  { state: 'hidden' | 'minimized' | 'small' }
```

Pone la caja del chat expandida, esto es abierta y visible.

*Ejemplo (maximizar la caja de chat):*
```javascript
  chatApp.executeMethod('setDisplayState', { state: 'small' })
```

Minimiza la caja del chat, esto es cerrado pero visible.

*Ejemplo (minimizar la caja de chat):*
```javascript
  chatApp.executeMethod('setDisplayState', { state: 'minimize' })
```

*Ejemplo (ocutlar la caja de chat):*

Oculta la caja del chat sin destruir la sesión que tenga abierta.

```javascript
  chatApp.executeMethod('setDisplayState', { state: 'hidden' })
```

### *addMetadata*

Una vez que el chat está instalado y activo en la web se le puede enviar metadata que será agregada a la conversación en curso o bien a una futura que se pudiera iniciar en la sesión. Esta metadata puede ser una serie de valores definidos como par `clave: valor`. Estos valores quedarán disponibles en los reportes de las conversaciones.

#### params
```javascript
{
  [string]: any
}
```


*Ejemplo (Agregar 2 campos de metadata):*
```javascript
  chatApp.executeMethod('addMetadata', { esVip: true, sucursal: 'Providencia' })
```


### *addMessage*

Permite enviar un mensaje (en nombre del usuario actualmente logueado) al agente que esta atendiendo la conversación.

!> **Importante** Si se invoca a `addMessage` antes que exista una conversación creada, entones esta llamada tendrá el efecto de crear una conversación a partir de este primer mensaje.


#### params

```javascript
  {
    content: String,
    type: "NORMAL" | "SYSTEM",
  }
```

*Ejemplo (Enviar un mensaje normal - visible en el chat):*
```javascript
  chatApp.executeMethod('addMessage', { content: 'Quiero solicitar un crédito', type: 'NORMAL' })
```

*Ejemplo (Enviar un mensaje de sistema - no visible en el chat):*
```javascript
  chatApp.executeMethod('addMessage', { content: 'El usuario es de tipo VIP - Atento!', type: 'SYSTEM' })
```

### *setInquiry*

Define el _Inquiry_ para el contexto de creación de una conversación. Sólo será efectivo si se inicia una conversación. Es necesario conocer con anterioridad el `id` del _Inquiry_ deseado. 

?> _TIP_ Para limpiar el _Inquiry_ definido se puede usar el mismo método con argumento `0`.


#### params

```javascript
  {
    inquiryId: Number,
  }
```

*Ejemplo (ELegir el inquiry de id=123):*
```javascript
  chatApp.executeMethod('setInquiry', { inquiryId: 123 })
```

*Ejemplo (Limpiar el inquiry previamente elegido):*
```javascript
  chatApp.executeMethod('setInquiry', { inquiryId: 0 })
```

### startConversation

Comienza una nueva conversación de forma programática. Recibe un objecto con los datos iniciales para iniciar una conversación.
`inquiry` (opcional): Un entero que es el id del inquiry (Solicitud) con la que queremos asociar la conversacion
`message` (opcional): Un object que tiene que tener 2 campos obligatorios: `content` y `content_type`. Lo que se complete en content sera el texto que es enviado como primer mensaje de la conversación.
`metadata` (opcional): Un object que tiene clave/valor que permite agregar metadata a la conversación.


#### params

```typescript
  {
    inquiry_id?: Number,
    message?: { 
      content: String,
      content_type: "text/plain" 
    }, 
    metadata?: Object;
  }
```
*Ejemplo (Iniciar una conversación con un mensaje):*
```javascript
  chatApp.executeMethod('startConversation', {
    message: { 
      content: "Primer mensaje de la conversación", 
      content_type: "text/plain" 
    }, 
  });
```


*Ejemplo (Iniciar una conversación con un mensaje, metadata y un inquiry ya seleccionado):*
```javascript
  chatApp.executeMethod('startConversation', {
    inquiry_id: 34,
    message: { 
      content: "Primer mensaje de la conversación", 
      content_type: "text/plain" 
    }, 
    metadata: { 
      vip: true, 
      golden: "yes"
    }
  });
```

### logout

Cierra la sesion del usuario que esta teniendo una conversación.

*Ejemplo:*
```javascript
  chatApp.executeMethod('logout');
```