# Índice

1. [Introducción](#introducción) - Breve explicación sobre qué es un evento.
2. [Utilización](#utilización) - Cómo se utiliza un evento.
3. [Lista de eventos](#lista-de-eventos) - Una lista completa con cada uno de los eventos de JDA.
    - [Tipos de eventos](###tipos-de-eventos) - Todas los diferentes categorías de los eventos de JDA.
4. [Conclusión](#conclusión) - Corto resumen de todo lo explicado en esta guía

## Introducción
Los eventos son funciones que se utilizan para que cuando Discord envíe una información al bot, esa información pueda ser manipulada por el programador o usuario que está haciendo uso del bot.

---

## Utilización
Primero, debemos crear una *package* para tener un mayor orden y este será llamado `eventos` (o *events*), cada clase que creemos ahí, tendrá que empezar obligatoriamente por un:
```java
package eventos; // O events
// Aunque si está dentro de otro package también, se deberá colocar totalmente el package, es decir:
package algo.algo.events;
```

Cada uno de los eventos tiene un parámetro en su función, que puede ser `GuildMessageReceivedEvent` (en el evento, en el que envían un mensaje de un servidor), el nombre de esa función será lo mismo pero le quitamos al final la palabra *Event* y agregamos al principio un *on*, quedando así como resultado `onGuildMessageReceived`, este será el nombre de la función en la cual se manejarán todos los mensajes que vengan de un servidor.

Dentro del *package eventos/events* creamos una clase con un nombre descriptivo del evento, si trata de mensajes podemos colocar `Mensajes` (o *Messages*) seguido de un `Listener` que será otra clase que se debe extender por la clase, es decir, la clase que estamos creando va a ser extendida por el *Listener*, para eso utilizamos estas líneas de código:
```java
import net.dv8tion.jda.api.hooks.ListenerAdapter;

public class NombreDescriptivo extends ListenerAdapter {

}
```
Solamente reemplazamos el *NombreDescriptivo*, probablemente por el de nuestra elección (*MessagesListener*), dentro de esta clase podemos colocar muchos eventos pero para mantener un orden, colocaremos solo uno para ser distinguido de todos los demás.

Como se ha escrito anteriormente, el nombre de la función para tener los mensajes, será `onGuildMessageReceived` y la función, será pública y no devolverá nada, esa función también como se ha leído posteriormente, recolectará un parámetro llamado `GuildMessageReceivedEvent` al cual le podemos asignar cualquier nombre, por lo general, ese nombre es `event`, `evento` o simplement `e`. Lo anteriormente dicho sería en código, lo siguiente:
```java
public void onGuildMessageReceived(GuildMessageReceivedEvent e) {

}
```
El valor de la variable `e`, es de tipo `GuildMessageReceivedEvent` y este contiene muchas funciones que por ejemplo, pueden ser:
`e.getMessage()`, `e.getChannel()`, `e.getAuthor()`, `e.getMessageId()`, `e.isWebhookMessage()`, `e.getMember()` y esas funciones devuelven otras cosas que también devuelven otras cosas, como: `e.getMessage().getContentRaw()` que devolverá el contenido tal cual fue enviado a Discord.

Con todo eso, podemos hacer que por cada mensaje que el bot reciba, en la consola muestre el nombre del autor, canal, servidor y su contenido. Eso lo hacemos con la siguiente línea de código:
```java
System.out.println(String.format("%s. En el servidor %s, en el canal #%s, envió: %s", e.getAuthor().getAsTag(), e.getGuild().getName(), e.getChannel().getName(), e.getMessage().getContentRaw()));
```
Lo que si envío *"Holaaa!"* en el canal *#general* del servidor *Script Hub*, en la consola debería salir: *"Deivid#0045. En el servidor Script Hub, en el canal #general, envió: Holaaa!"*.

**Pero**, falta una pequeña cosa que es agregar el evento al bot para que lo detecte y pueda enviar las cosas por medio de las funciones, y para eso se utiliza una función del JDA o de la variable de tipo JDA que es el bot que probablemente tiene el nombre de tu bot, también `client` o `bot`, la función se llama `addEventListener()` y se debe añadir en el archivo principal o `Main`, todo esto escrito sería:
```java
import ejem.plo.events.*;
//......
bot.addEventListener(new MessageListener());
```

Uniendo cada una de las cosas dichas y añadiendo un breve comentario en cada una de las cosas, quedarían las siguientes líneas de código:
```java
package ejem.plo.eventos; // O ejem.plo.events, o también events solo

import net.dv8tion.jda.api.hooks.ListenerAdapter; // Importamos la clase que será extendida, la cual es: ListenerAdapter

public class MessageListener extends ListenerAdapter { // Definimos la clase como pública y con el nombre: MessageListener
    public void onGuildMessageReceived(GuildMessageReceivedEvent e) { // Inicio del evento en el cual se recibirán los mensajes provenientes únicamente de un servidor (Guild)
        // Y se muestra en la consola el nombre del autor con su tag, el servidor y canal en el cual se envió el mensaje, y el contenido del mensaje
        System.out.println(String.format("%s. En el servidor %s, en el canal #%s, envió: %s", e.getAuthor().getAsTag(), e.getGuild().getName(), e.getChannel().getName(), e.getMessage().getContentRaw()));
    }
}
```
```java
//-------- En el archivo principal o Main, se debe(n) importar el/los evento(s) y justo después del .build():
bot.addEventListener(new MessageListener()); // Se reemplaza la variable bot por la que se creó al principio en el archivo principal
```

**¡Y listo!** Toda esta operación la debemos repetir por cada evento que querramos crear y/o manejar sus datos.

---

## Lista de eventos
Una lista de todos los eventos de JDA, [aparece aquí](https://github.com/DV8FromTheWorld/JDA/wiki/8%29-List-of-Events) aunque no está todavía para la versión `4.0.0_39` de JDA y son demasiadas las que aparecen.

Aquí van los eventos más importantes de JDA, cada uno de los eventos con el nombre de la función seguido del nombre del parámetro y una breve descripción:
```
onReady (ReadyEvent) - Cuando el bot está listo

onGenericSelfUpdate (GenericSelfUpdateEvent) - Cuando se cambia alguna información del bot
onGenericUserUpdate (GenericUserUpdateEvent) - Cuando se cambia alguna información de cualquier usuario
onGenericUserPresence (GenericUserPresenceEvent) - Cuando se cambia la presencia de un usuario
onUserTyping (UserTypingEvent) - Cuando un usuario empieza a hablar

onMessageDelete (MessageDeleteEvent) - Cuando se elimina un mensaje
onMessageEmbed (MessageEmbedEvent) - Cuando se recibe un mensaje embed
onMessageReceived (MessageReceivedEvent) - Cuando se recibe cualquier mensaje
onMessageUpdate (MessageUpdateEvent) - Cuando se edita un mensaje
onMessageReactionAdd (MessageReactionAddEvent) - Cuando se reacciona a un mensaje
onMessageReactionRemoveAll (MessageReactionRemoveAllEvent) - Cuando se quitan todas las reacciones de un mensaje
onMessageReactionRemove (MessageReactionRemoveEvent) - Cuando quita la reacción a un mensaje
-- Si le agregamos un "Private" al principio de estos, son los mensajes privados
-- Si agregmos un "Guild" al principio de esos, son los mensajes de un servidor

onGuildMemberJoin (GuildMemberJoinEvent) - Cuando entra un miembro a un servidor
onGuildMemberLeave (GuildMemberLeaveEvent) - Cuando un miembro se sale del servidor
onGuildMemberNickChange (GuildMemberNickChangeEvent) - Cuando se cambia el apodo a un miembro
onGuildMemberRoleAdd (GuildMemberRoleAddEvent) - Cuando se añade un rol a un miembro
onGuildMemberRoleRemove (GuildMemberRoleRemoveEvent) - Cuando se le quita un rol a un miembro

onGenericGuildUpdate (GenericGuildUpdateEvent) - Cuando cambia cualquier parámetro del servidor

onGuildVoiceDeafen (GuildVoiceDeafenEvent) - Cuando se ensordece a alguien del servidor
onGuildVoiceMute (GuildVoiceMuteEvent) - Cuando se mutea a alguien del servidor
onGuildVoiceJoin (GuildVoiceJoinEvent) - Cuando alguien entra a un canal de voz
onGuildVoiceLeave (GuildVoiceLeaveEvent) - Cuando alguien sale del canal de voz

onTextChannelCreate (TextChannelCreateEvent) - Cuando se crea un canal de texto
onTextChannelDelete (TextChannelDeleteEvent) - Cuando se elimina un canal de texto
onTextChannelUpdatePermissions (TextChannelUpdatePermissionsEvent) - Cuando se actualizan los permisos de un canal de texto
onGenericTextChannelUpdate (GenericTextChannelUpdateEvent) - Cuando se cambia cualquier otra cosa del canal de texto

onVoiceChannelCreate (VoiceChannelCreateEvent) - Cuando se crea un canal de voz
onVoiceChannelDelete (VoiceChannelDeleteEvent) - Cuando se elimina un canal de voz
onVoiceChannelUpdatePermissions (VoiceChannelUpdatePermissionsEvent) - Cuando se actualizan los permisos de un canal de voz
onGenericVoiceChannelUpdate (GenericVoiceChannelUpdateEvent) - Cuando se cambia cualquier otra cosa del canal de voz

onCategoryCreate (CategoryCreateEvent) - Cuando se crea una categoría
onCategoryDelete (CategoryDeleteEvent) - Cuando se elimina una categoría
onCategoryUpdatePermissions (CategoryUpdatePermissionsEvent) - Cuando se actualizan los permisos de una categoría
onGenericCategoryUpdate (GenericCategoryUpdateEvent) - Cuando se cambia cualquier otra cosa de la categoría

onRoleCreate (RoleCreateEvent) - Cuando se crea un rol
onRoleDelete (RoleDeleteEvent) - Cuando se elimina un rol
onGenericRoleUpdate (GenericRoleUpdateEvent) - Cuando se cambia cualquier cosa del rol

onEmoteAdded (EmoteAddedEvent) - Cuando se añade un emoji
onEmoteRemovedEvent (EmoteRemovedEvent) - Cuando se elimina un emoji
onGenericEmoteUpdate (GenericEmoteUpdateEvent) - Cuando se cambia cualquier cosa del emoji
```

### Tipos de eventos
Hay diferentes tipos de eventos, los de canales, servidores, mensajes, etc.
Para identificar cada uno de esos, solamente miramos el principio de alguno de estos y verificamos qué tipo de eventos es:
- `Generic > Abarca todo lo que sigue después de eso`.
- `User > Usuarios`.
- `Message > Mensajes`.
- `Guild > Servidor`.
- `Voice > Voz`.
- `TextChannel > Canal de texto`.
- `VoiceChannel > Canal de voz`.
- `Category > Categoría`.
- `Role > Rol`.
- `Emote > Emoji`.

---

## Conclusión
Los eventos sirven para que nosotros como programadores, manipulemos la información que nos llega de Discord y la podamos representar de alguna forma o que el usuario haga otra cosa con eso.
Creamos un *package* y dentro una clase con un nombre descriptivo de la información que obtiene el bot y colocamos una función pública que no devuelve nada con el nombre del evento y que extienda a *ListenerAdapter* y esa función tiene un parámetro que es el que devuelve el evento.
Añadimos en el archivo principal o `Main`, la función de la variable `bot` con tipo JDA, cuya función es llamda `addEventListener` que añade el evento al bot para que envíe todo ahí.

<font size=4>**¡Muchas gracias por leer! Continúa con el uso de un administrador de comandos** (*command handler*) **, [aquí](/java/command-handler.md)**</font>
