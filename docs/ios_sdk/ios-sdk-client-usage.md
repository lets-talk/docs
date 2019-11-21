# Avanzado

También es posible bajar el nivel de abstracción.

## Registra al usuario
Para registrar al usuario, debes crear una instancia de LetsTalkSessionManager:
```
    let session = LetsTalkSessionManager(
        longSubdomain: "springfield",
        credentials: .guestClient(
            name: "Homer J. Simpson",
            email: "chunkylover53@aol.com"
        ),
        userInfo: nil
    )!
    // session es opcional porque podrías poner un string inválido en el subdominio
```
Existen varios tipos de credenciales:
```
    public enum Credentials {
        // Si el usuario ya está logueado, usas este
        // Para generar este token, es necesaria una integración especial con nuestro software
        case client(token: String, provider: String)
        
        // Si la app no requiere crearse una cuenta
        case guestClient(name: String, email: String)
        
        // Si estás ocupando el SDK para hacer una app para agentes
        case agent(username: String, password: String)
    }
```
## Escoge una conversación que quieras abrir
```
    // Este objeto representa la conversación que queremos abrir
    let conversation = ConversationIdentifier.unexisting(
        description: .inquiryless(
            title: "Conversación",
            greeting: "Hola, ¿en qué te podemos ayudar?",
            initialMessages: nil
        )
    )
```
Para esto tienes varias opciones:
```
    enum ConversationIdentifier {
        // Si conoces el id de una conversación que ya existe, usas este:
        case existing(conversationId: Int)
        
        // Si ya descargaste una conversación, usando otra de las opciones avanzadas, usas este:
        case downloaded(conversation: Conversation)
        
        // Y si lo que quieres es crear una nueva conversación, usas este:
        case unexisting(description: ConversationDescription)
    }
```
En caso de querer crear una conversación, necesitas describir qué tipo de conversación es la que necesitas:
```
    enum ConversationDescription {
        // Si tu organización no ocupa inquiries, usas este:
        case inquiryless(title: String, greeting: String, initialMessages: SendableMessage.Content?)
        
        // Si tu organización ocupa inquiries y ya descargaste la descripción de uno, usas este:
        case externalInquiry(inquiry: InquiryData, initialMessages: SendableMessage.Content?)
        
        // Estas opciones son solo para apps para agentes:
        case internalInquiry(inquiryId: Int, inquiryName: String, membersIds: [Int])
        case directClientContact(clientId: Int, membersIds: [Int])
        case directUserContact(userId: Int, membersIds: [Int])
        case groupConversation(groupId: Int, groupName: String)
    }
```
## Abre una ventana de chat
```
    // Este objeto representa la conversación que queremos abrir
    let conversation = ConversationIdentifier.unexisting(
        description: .inquiryless(
            title: "Conversación",
            greeting: "Hola, ¿en qué te podemos ayudar?",
            initialMessages: nil
        )
    )
    
    // Este objeto maneja los datos de la conversación
    let conversationManager = ConversationManager(
        conversation: conversation,
        sessionManager: session,
        externalNotificationSignal: nil
    )
    
    // Este es el ViewController que proveemos nosotros
    // Pero usando directamente el conversationManager, se puede construir uno completamente distinto
    let chatViewController = JsqChatViewController(conversationManager: conversationManager)
```
## Obten la lista de conversaciones que ha tenido el usuario
Si deseas soportar que el cliente pueda tener múltiples conversaciones simultaneamente,
puedes descargar la lista de conversaciones actuales
```
    let conversationListProvider = PaginatedDataProvider<Conversation>.default(session: session)
    let currentConversationList: FullData<Conversation> = conversationListProvider.latest.value
    print(currentConversationList)
```
## Obten la lista de inquiries que puede seleccionar el usuario
Si deseas mostrarle al usuario distintas opciones para iniciar una conversación,
puedes descargar la lista de inquiries más reciente
```
    let inquiryListProvider = Inquiries.Provider.default(session: session)
    let currentInquiryList: [Inquiries.Inquiry] = inquiryListProvider.latest.value
    print(currentInquiryList)
```
## Provee tus propios textos y traducciones
Nosotros proveemos el SDK en inglés y castellano, pero puedes agregar más idiomas,
o cambiar el texto para calzar mejor con la imagen que deseas presentar a tus usuarios.

Para customizar esto, solo debes crear un archivo llamado `ChatSDK.strings`
y agregarlo a tu aplicación de tal forma que quede dentro del `main bundle`.
Con ayuda de Xcode puedes localizar este archivo para incluír todos los idiomas que necesites.

Si pienzas modificar estos textos, te recomendamos usar nuestros archivos como base,
los pueedes encontrar en `Localization/en.lproj/ChatSDK.strings` y `Localization/es-419.lproj/ChatSDK.strings`.

Por ejemplo, si quisieras darle un tono más coloquial a tu aplicación, podrías agregar esto a tu `ChatSDK.strings`

    // Esta es la línea original en `Localization/en.lproj/ChatSDK.strings`
    // "error_sending.reason" = "Hubo un error al enviar este mensaje, ";
    "error_sending.reason" = "Pucha que lata, no se pudo mandar tu mensaje";

O al contrario, si lo que planeas es darle un tono muy serio y técnico

    "error_sending.reason" = "Se produjo un fallo en la comunicación con el servidor al intentar enviar este mensaje";

Esta es la lista completa de `strings` que puedes editar en la versión actual del SDK:
```
    // En caso de fallar el envío de un mensaje:
    error_sending.reason
    error_sending.try_again_question
    error_sending.try_again_yes
    error_sending.try_again_no
    
    // Si el usuario hace click en un mapa,
    // se le da la opción de abrirlo en Maps o Google Maps si lo tiene instalado
    share_location.open_in_maps
    share_location.open_in_google_maps
    share_location.cancel
    
    // En el caso de que por algún problema de configuración
    // no sea posible determinar el título correcto para la conversación
    // se usará este título en la barra de navegación
    conversation_default_title
    
    // Esto es sólo importante para aplicaciones para agentes:
    kicked_out.reason
    kicked_out.stay_question
    kicked_out.stay_as_viewer
    kicked_out.stay_but_errored
    kicked_out.stay_no
    send_internal_message
```
