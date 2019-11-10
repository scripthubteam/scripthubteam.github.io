# Índice

1. [Introducción](#introducción) - Introducción al administrador de comandos o *command handler*.
2. [Adminisitrar comandos](#administrar-comandos) - Cómo administrar los comandos y tener un código semi-estático que mueva todo.
3. [Crear comandos](#crear-comandos) - Cómo crear los comandos con toda la información necesaria.
4. [Conclusión](#conclusión) - Resumen de todo sobre el administrador de comandos.

## Introducción
Un administrador de comandos o *command handler* es aquel que por medio de archivos separados y manteniendo un orden, ejecuta siempre el mismo código el cual solamente busca el archivo y lo ejecuta como si de una librería o archivo externo se tratase.

---

## Administrar comandos
Ya sabiendo qué es un evento y los tipos de eventos, hacemos un evento que detecte los mensajes con lo siguiente:
```java
public void onGuildMessageReceived(GuildMessageReceivedEvent e) {

}
```
Ahora debemos verificar varias cosas:
- **Si el que envió el mensaje es un bot**,
Comprobamos esto porque puede ocurrir que alguien use un comando que repita lo del usuario y el bot responda, lo que puede ocacionar algo absurdo y la gente se confundirá, lo mismo pasa si está en el propio bot, porque lo repite.
- **Si el mensaje empieza con el prefijo del bot**,
Examinamos si el mensaje empieza con el prefijo del bot para ver si es un comando o no, esto sirve porque puede ser que el mensaje empiece por lo que sea y aún así lo detecta como comando.

Para eso, colocamos estas líneas de código:
```java
if (e.getMessage().getAuthor().isBot()) return;
if (!e.getMessage().getContentRaw().startsWith("PREFIX")) return;
```
Reemplazamos obviamente `PREFIX` por el respectivo prefix del bot.
Ahora debemos establecer los argumentos de un comando, para eso se quitan las primeras letras del mensaje, es decir, el prefijo y se divide el mensaje por todos los espacios usando una expresión regular (*RegEx*) la cual es la siguiente: `" +"`. Todo esto escrito, sería lo siguiente:
```java
String[] args = e.getMessage().getContentRaw().substring("Prefijo").trim().split(" +");
```
Pero falta una cosa, quitar el primer elemento del array que sería el comando, pero todavía no lo vamos a hacer. Vamos por partes: primero, verificamos si hay comando por medio de la lectura de la longitud de los argumentos, después se define el comando y por último, se redefinen los argumentos quitando el primero:
```java
if (args.length < 1) return;
String cmd = args[0].toLowerCase();
args = Arrays.copyOfRange(args, 1, args.length);
```

Ya por último, viene lo más simple que sería ir a medida agregando varios `if`s o un `switch` en el cual se ejecute un comando u otro. Podemos utilizar un simple `switch` de la siguiente manera:
```java
switch (cmd) {
  case "nombreComando":
    comando.exec(e.getJDA(), e.getMessage(), args);
    break;
  case "otroComando":
    otroCmd.exec(e.getJDA(), e.getMessage(), args);
    break;
}
```
**¡Pero eso es todo por aquí! En la sección siguiente se muestra como crear el comando y de donde viene esa función de ejecución (*exec*) y cuáles son los parámetros.**

## Crear comandos
Debemos crear un *package* llamado *comandos*, *cmds*, *commands* o de la manera que ustedes prefieran, donde irán todos nuestros comandos que serán una clase con una función de ejecución llamada *exec* y una visualización de esto sería así, con el comando de *ping*:
```java
package ejem.plo.cmds;

import net.dv8tion.jda.api.JDA;
import net.dv8tion.jda.api.entities.Message;

public class PingCommand {
    public static void exec(JDA bot, Message msg, String[] args) {
        msg.getChannel().sendMessage(String.format("Pong! %sms.", (int)bot.getGatewayPing())).queue();
    }
}
```
Como nos dimos cuenta en ese ejemplo, tenemos al bot, mensaje y sus argumentos en un *Array* de *Strings* (*String[]*) y para enviar un mensaje se obtiene el canal y se crea el mensaje con la función de *sendMessage* y se agrega a la *queue* o lista de mensajes a enviar. Es decir, todo eso es:
```java
msg.getChannel().sendMessage("Holaaa!").queue();
// Se obtiene el canal de donde fue enviado el mensaje, se crea el mensaje a enviar y se coloca en la lista de los mensajes a enviar para ser enviado a continuación.
```

---

## Conclusión
Aprendimos a crear un sistema de comandos detectando el comando, si es un bot no haga nada, si no empieza por el prefix no haga nada y sus argumentos, también a exportar cosas de los diferentes comandos.

<font size=4>Eso es todo... Hasta aquí llega la guía de JDA. **¡Muchas gracias por leer!**</font>
