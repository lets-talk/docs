# Instalación Avanzada de Apps de Let’s Talk

Además de la opción básica, las apps aceptan una serie de parámetros adicionales para la customización gráfica, pero también para la operación.

## Inicialización de la Libreria de Letstalk

```javascript
window.LetstalkSettings = {
  organization: '[ORGANIZATION]',
}

window.$Letstalk.on('ready', function() {
  // IMPORTANTE SOLO USAR LIBRERIA DENTRO DE ESTA FUNCION
});

```

## Variables de Instalación
En las distintas fases y versiones de instalación de hace referencia a variables entre paréntesis cuadrados `[]`, esto quiere decir que se deben usar variables propias de la instancia a implementar. En caso de no contar con estas variables comunicarse con la/el encargada/o del proyecto o con soporte@ltmessenger.com.

```
[ORGANIZATION]: El nombre de su organizacion
[APP_NAME]: El nombre del chat que fue creado
[KEY]: Key para instancias privadas
[TOKEN]: Token para instancias privadas
```

## Sesión de usuario (Utilizacion en sitio privado)

En particular, para sitios que posean una **sesión con usuario identificado**, es decir, donde ya se sabe quién es el visitante la app de chat permite ser inicializada con una sesión activa para el visitante autorizada por las credenciales de la misma organización. Esta funcionalidad es útil cuando se usa la app de chat en sitios privados como: intranet, web internas con usuarios con sesión iniciada.

A cada organización se le provee una pareja **(key, token)** de credenciales que permite integrar su propia logica de authenticación de usuarios. Si no posee esta información deberá solicitarlo escribiendo a **soporte@letstalk.com**.
Deberá proveer dicha pareja al momento de la inicialización del chat.

Al inicializar la app de chat con **consumer** setting y **visitor** **el visitante no tendrá que iniciar sesión** en la app de chat.

*Ejemplo*:

```javascript
var settingsChatPrivado = {
  consumer: {
      key:   '[KEY]',
      token: '[TOKEN]'
  },
  visitor: {
      name: 'Nombre Cliente',
      email: 'email@gmail.com',
      attrs: {
        sucursal: 'Sucursal Providencia',
        tipoBanca: 'Banca Personal',
        convenio: 'Convenio Pyme',
      }
  }
};

window.LetstalkSettings = {
  organization: '[ORGANIZATION]',
}

window.$Letstalk.on('ready', function() {
  // Abrir App de chat que se llama 'chat-sitio-privado'
  window.$Letstalk.launch('[APP_NAME]', settingsChatPrivado).then((app) => {
     app.on('ready', function() {
        app.executeMethod('setDisplayState', { state: 'small' });
     });
  });
});

```

## Atributos del usuario

Además de poder iniciar la app de chat con un usuario logueado podrá agregar información al perfil del usuario pasando datos extra en el objeto **visitor.attrs**. Esta información puede ser muy valiosa para el agente que reciba la conversación ya que le brinda mayor contexto sobre con quien va a interactuar.

Para esto se debe entregar un objeto **visitor** que sólo será aceptado si viene con las credenciales correctas del consumer (la organización) como se vió en el ejemplo anterior.
Los valores para **name** y **email** son requeridos en el objecto visitor, mientras que attrs puede omitirse.

El uso esperado es que la web de la organización complete el objeto visitor con los datos necesarios antes de inicializar la app de chat.

## Atributos del contexto

En la inicialización de una app también es posible adjuntar metadata que son parámetros personalizados que se mostrarán como un mensaje interno al inicio de la conversación. El objetivo es brindar información relevante al agente que atenderá al cliente.

*Ejemplo:*

```javascript
window.$Letstalk.launch('[APP_NAME]', {
  metadata: {
    empresa: window.location.href.match(/pyme/) ? 'pyme': 'no-pyme',
  }
})
```

En el ejemplo se envía el atributo empresa con el valor pyme o no-pyme dependiendo de la url de carga dla app de chat, lo que indicará al agente si se inició la conversación en el portal pyme o no-pyme.


Para definir una lista de filtros deben solicitar su configuración a soporte@ltmessenger.com.
