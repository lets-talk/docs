## Iniciar Chat mediante click en elemento de la web
*(El ejemplo explica como mostrar un botón en su sitio web dependiendo de la
  configuración horaria del widget. También se muestra como desplegar
  el chat cuando se hace click en dicho botón)*

Un caso típico de uso es iniciar el widget oculto y sólo mostrar luego que el usuario realiza alguna interacción en su sitio web (por ejemplo hacer click en un botón).
En el siguiente ejemplo se muestra como lograr este resultado. Se utilizará el evento *[api.events.availabe](apievents#apieventsavailable)* para determinar si mostrar o no un botón que permite mostrar el widget sólo si el mismo se encuentra dentro del horario configurado para mostrarse.

[cinwell website](//codepen.io/sandinosaso/embed/preview/MVpENw/?height=500&theme-id=dark&default-tab=js,result&embed-version=2 ':include :type=iframe width=100% height=550px')

## Explicación paso a paso

Ni bien se carga la pagina ejecutamos la inicialización del widget invocando la
función `window.$Letstalk.init()` esta función recibe como parametro un objecto con la configuración inicial requerida para levantar las distintas apps:

```javascript
  window.$Letstalk.init({
    // Inicializacion del launcher objeto de incializacion
    environment: {},
    initialData: {},
  });
```

Podemos agregar data inicial a una app en particular (pueden haber muchas definidas).
En este ejemplo supongamos tengamos una app de chat que se llama `chat-sitio-publico`,
si quisieramos que este chat parta oculto podriamos hacer:

```javascript
   window.$Letstalk.init({
    // Inicializacion del launcher objeto de incializacion
    environment: {},
    initialData: {
      'chat-sitio-publico': {
        display: 'hidden',
      }
    },
  });
```

Finalmente definimos una función que se ejecute cuando cuando el widget determina
si debe mostrarse o no (según su configuración de horario). Dicha función no hace mas que mostrar el botón que permitirá al usuario desplegar el chat de ayuda.

```javascript
  messenger.on('api.events.available', function (data) {
    $('#openChat').show();
  });
```

Solo resta mostrar el chat cuando el usuario haga click en el botón de **Ayuda en Línea**. Para ello ejecutamos una función que invoca el método *[executeAppMethod](publicapi#windowltsetdisplaystatestring)* que le dice a la app que ejecute su metodo setDisplayState para mostrarse abierta :

```javascript
  $('#openChat').on('click', function() {
    window.$Letstalk.executeAppMethod('chat-sitio-publico', 'setDisplayState', { state: 'minimized' });
  });
```
