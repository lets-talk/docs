# AppsSDK disponible para creadores de Apps

## Como usar

```javascript
  import { SDK } from '@lets-talk/apps-sdk';

  const AppsSDK = new SDK();
```


## *getAppSettings(): Promise*

Obtiene las settings de una app. Tener en cuenta que estas settings son las que por defecto se configuraron por la app, una forma de sobrescribirlas facilmente es hacer:

*Ejemplo:*
```javascript
    AppsSDK.getAppSettings().then((event) => {
      const { data } = event;
      const { initialData } = data;
      
      const configuration = JSON.parse(data.payload);
      const mergedConfiguration = initialData ? { ...configuration, ...initialData.payload } : configuration;
    }).catch((error) => {
      console.error('Error getting my settings:', error);
    });
```

En el ejemplo de arriba se pisan las settings que hubiera con lo que venga en initialData (el que me abrió me paso data inicial) 

## *notify(eventName: string, data: any): Promise*

Permite a una aplicación notificar que un evento ocurrió. Si hay alguna regla que abre otra app y que depende de mi evento, esta regla es evaluada con la data que proporciono como segundo argumento de la función.

*Ejemplo (nofiticar que la App esta ready):*
```javascript
  AppsSDK.notify('ready')
```

*Ejemplo (notificar custom event):*
```javascript
  AppsSDK.notify('lt.appEvent.gotClientValues', [
    {
      key: 'key1',
      value: { user_name: 'User' }
    }
  ])
```


## *addEventHandlers(Object): Promise*

Permite a nuestra App exponer metodos que se pueden ser invocados por otras apps o desde donde esta app este instalada.

Aqui se pasa un objeto cuyas `keys` son el nombre del metodo que exponemos, y cuyos `values` son funciones (callback) que manejan dicho evento.

*Ejemplo (Una app de chat podria manejar estos eventos):*
```javascript
AppsSDK.addEventHandlers({
    addMessage: (args) => dispatch(publicApiAddMessage(args)),
    addMetadata: (args) => dispatch(publicApiAddConversationMetadata(args)),
    logout: () => dispatch(publicApiLogout()),
    setDisplayState: (args) => dispatch(publicApiSetDisplayState(args.state)),
    setInquiry: (args) => dispatch(publicApiSetInquiry(args.inquiryId)),
    startConversation: (args) => dispatch(publicApiStartConversation(args)),
  });
```

## *on(eventName: string, handler: Function): Promise*

Permite a nuestra App reaccionar a eventos que le son notificados desde donde este instalada (por el launcher)

```javascript
  AppsSDK.on('notify-app-of-launcher-event', (event) => {
    const { data } = event;
    const { type, payload } = data;
    switch (type) {
      case 'dom.events.visibilitychange':
        dispatch(appsSdkVisibilityChangeAction(payload));
        break;
      case 'dom.events.beforeunload':
        dispatch(appsSdkBeforeUnloadAction(payload));
        break;
      case 'user.events.activity': {
        const actionToDispatch = payload.active === true ? clientIsActiveInAppAction : clientIsInactiveInAppAction;
        dispatch(actionToDispatch());
        break;
      }

      default:
        break;
    }
  });
```