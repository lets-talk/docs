# AppsSKD - Chat Api Metodos Publicos

!> **Importante** todos los metodos que se muestran aquí estan disponibles una vez cargada la libreria de Letstalk, por lo que hay que asegurarse de ejecutarlos dentro del evento `ready`

```javascript
  window.$Letstalk.on('ready', function() {
    window.$Letatalk.(launch|remove|listApps|isAvailable|executeAppMethod)
  });
```

## *launch*
#### *launch (appName: string, appSettings: Object): Promise*

Permite lanzar una App (ej: una App de Chat)

*Ejemplo simple (configuración por defecto):*
```javascript
  window.$Letstalk.launch('[APP_NAME]');
```

*Ejemplo avanzado (configuración basica):*
```javascript
  window.$Letstalk.launch('[APP_NAME]', { display: 'minimized' });
```

Cuando se lanza una app se puede esperar esperar a que esta avise que se cargo para invocarle metodos, por ejemplos si quisieramos que una aplicación apenas se lance se maximize

```javascript
  window.$Letstalk.launch('[APP_NAME]').then((app) => {
     app.on('ready', function() {
       // En este momento se le pueden ejectura acciones a la App
        app.executeMethod('setDisplayState', { state: 'small' });
     });
  });
```

## *remove*
#### *remove (appName: string): Promise*

Permite cerrar la app. Devuelve una promesa que se resuelve si se ejecuto correctamente (el manejador de apps recibió la petición)

*Ejemplo:*
```javascript
  window.$Letstalk.remove('[APP_NAME]')
    .then(() => { console.log('App removed') })
    .catch((e) => console.log('Error removing app:', e))
```

## *isAvailable*
#### *isAvailable (appName: string): Promise*

Permite saber si una App esta disponible (cumple las reglas de disponibilidad en este instante). Devuelve una promesa que se resuelve si la App esta disponible y se hace reject en caso de que no este disponible

*Ejemplo:*
```javascript
  window.$Letstalk.isAvailable('[APP_NAME]')
    .then(() => { console.log('App is available') })
    .catch((e) => console.log('App is not available:', e))
```

## *on*
#### *on (eventName: string, callback: Function): Promise*

Permite ejecutar una funcion cuando determinado evento ocurre. Para ver la lista de eventos consulte en la lista de eventos disponibles.

*Ejemplo:*
```javascript
  window.$Letstalk.on('[EVENT_NAME]', function() {

  });
```

## *listApps*
#### *listApps (): Promise<string[]>*

Permite obtener la lista de todas las Apps definidas. 

*Ejemplo:*
```javascript
  window.$Letstalk.listApps().then((apps) => {
    console.log('Got the following apps:', apps);
  });
```

