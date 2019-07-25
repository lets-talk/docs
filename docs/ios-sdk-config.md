# Usando Let's Talk en iOS con el ChatSDK 0.5.0

El SDK soporta iOS 9 en adelante y está hecho en Swift 4.2.

Nota:
Si se utiliza una versión de Pods < 1.6 es necesario habilitar Legacy Build System en XCode
File -> Project Settings (or Workspace Settings)-> Build System

## Paso 1 - Instalar el SDK

Usando CocoaPods, se recomienda usarlo como un pod local.

Agrega el ChatSDK al Podfile y ejecuta pod install

    target :NombreDelTarget do
      pod 'ChatSDK', path: '../ChatSDK'
      
      ```
      # Algunas de nuestras dependencias, aún no tiene una versión en Swift 4.2
      # y como `cocoapods` aún no maneja automáticamente el uso de `SWIFT_VERSION`
      # es que en un `post install hook` debemos avisarle que esto sigue con Swift 4.1.
      #
      # Nótese que esto es solamente temporal
      # en una próxima versión ya no va a ser necesario para la instalación.
      #
      # Para saber más al respecto, puedes seguir este issue en github:
      # https://github.com/CocoaPods/CocoaPods/issues/7134
      #
      post_install do |installer|
        swift41pods = [
          'Anchorage',
          'Eureka',
          'LocationPickerViewController',
          'ReactiveCocoa',
          'PromiseKit',
          'ImagePicker',
          'ConsistencyManager',
        ]
    
        installer.pods_project.targets.each do |target|
          if swift41pods.include? target.name
            target.build_configurations.each do |config|
              config.build_settings['SWIFT_VERSION'] = '4.1'
            end
          end
        end
      end
    ```  
      # Existen un número de formas en que este setting puede quedar mal puesto,
      # si algo sucede y el proyecto falla al compilar,
      # se recomienda correr `pod deintegrate` y luego `pod install`.
      # Esto fuerza a regenerar los proyectos y recrear el entorno problemático.
    end

## Paso 2 - Actualiza el Info.plist

Asegúrate de que el Info.plist contenga las siguientes descripciones de uso

    Privacy - Location When In Use Usage Description
    Privacy - Camera Usage Description
    Privacy - Photo Library Usage Description
    
Esto lo requiere Apple porque el SDK permite a los usuarios mandar fotos y compartir su ubicación.
A los usuarios solo se les va a pedir los permisos relevantes cuando intenten compartir datos.

## Paso 3 - Registra al usuario
Antes de que puedan hablar, necesitas registrar al usuario con nosotros.

```
    LetsTalk.shared = LetsTalk(
        subdomain: "springfield",
        credentials: .guestClient(
            name: "Homer J. Simpson",
            email: "chunkylover53@aol.com"
        )
    )
```
Existen 3 tipos de credenciales que sirven para registrar clientes:

    // Para registrar un cliente que tiene una cuenta en tu aplicación
    case sponsoredClient(name: String, uid: String, uidType: String,
                         attrs: [String: String], key: String, token: String)

    // Para registrar un cliente que ha sido agregado en una integración con Let's Talk
    case client(token: String, provider: String)

    // Para registrar un cliente que no tiene una cuenta en nuestro servicio
    case guestClient(name: String, email: String)

## Paso 4 - Abre una ventana de chat
Pon un botón donde te acomode y configúralo para que cuando el usuario lo aprete,
se presente el ViewController del chat en tu stack de navegación.
```
    @IBAction func didPressChatButton(_ sender: Any) {
        let chatViewController = LetsTalk.shared.openLatestChatOrNew()
        navigationController?.pushViewController(chatViewController, animated: true)
    }
```
Alternativamente, abre la lista de conversaciones pasadas, que incluye un botón para crear nuevas conversaciones.
```
    @IBAction func didPressChatButton(_ sender: Any) {
        let chatViewController = LetsTalk.shared.openChatListAndMakeNew()
        navigationController?.pushViewController(chatViewController, animated: true)
    }
```
O si quieres prefieres mostrar la lista de inquiries para que el usuario decida con quién iniciar una conversación.
```
    @IBAction func didPressChatButton(_ sender: Any) {
        let inquirySelectorViewController = LetsTalk.shared.makeInquirySelector()
        navigationController?.pushViewController(inquirySelectorViewController, animated: true)
    }
```
---
