
- [Introducción](#introducción)
- [Requisitos](#requisitos)
- [Creando Bot](#creando-bot)
    - [Lanzador](#lanzador)
    - [Plugins](#plugins)
    - [Activación](#activación)
    - [Ayudas](#ayudas)

# Introducción

![Dificultad](https://img.shields.io/badge/Nivel-Intermedio%2FAvanzado-orange.svg)

Nuestro objetivo es crear un sistema que sea capaz de reconocer cualquier Clase que introduzcamos en un paquete, al que llamaremos *plugins*, como un plugin que podrá ser llamado por un comando con su mismo nombre.

De esta manera, implementar funciones a nuestro Bot será mucho más fácil ya que cada función será una Clase en sí misma.

__Ejemplo:__

![Ejemplo 1](https://i.imgur.com/4mYy8QL.png)
![Ejemplo 1](https://i.imgur.com/vkDj4ZQ.png)

# Requisitos

[![JDK](https://img.shields.io/badge/JDK-8+-red.svg)](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html "JDK Download")
[![Reflections](https://img.shields.io/badge/Reflections-Lastest-blue.svg)](https://github.com/ronmamo/reflections "Reflections GitHub")

Para este sistema vamos a necesitar la dependencia [Reflections](https://github.com/ronmamo/reflections) que puede ser fácilmente añadida a un proyecto Maven añadiendo lo siguiente en la etiqueta `<dependencies>`:

```xml
<dependency>
    <groupId>org.reflections</groupId>
    <artifactId>reflections</artifactId>
    <version>0.9.11</version>
</dependency>
```

Aunque desarrollaremos el sistema con JDK 8, este puede ser integrado en cualquier Bot escrito con un JDK superior.

# Creando Bot

## Lanzador

El modelo que vamos a crear tendrá dos clases controladoras del Bot, una que lo creará y una que lo ejecutará, simplemente para tener un sistema más ordenado. Por lo que es recomendado crear un paquete exclusivo para esas dos clases.

Lo primero será crear la clase del Bot, de manera que implemente EventListener y herede de ListenerAdapter.
Dentro de esta se creará un atributo estático de tipo JDA, que será nuestro bot creado con JDABuilder.

```Java
public class Bot
extends ListenerAdapter
implements EventListener{
    protected static JDA bot;

    // Método para ejecutar el Bot
    public void run()
    throws LoginException, RateLimitedException {
        JDABuilder builder = new JDABuilder(AccountType.BOT);
        builder.setToken("your-token");
        builder.addEventListener(new Bot());
        bot = builder.buildAsync();
    }

    // Método a ejecutar cuando el Bot inicie
    @Override
    public void onReady(ReadyEvent event) {
        System.out.println("\Activando bot...");
        System.out.println("Nombre: " + bot.getSelfUser().getName());
        System.out.println("ID: " + bot.getSelfUser().getId());
        System.out.println("¡Listo!");
        System.out.println("--------------");
    }

}
```

Una vez creada la clase, solo necesitamos llamarla desde una clase que generaremos como lanzador, simplemente para obtener un Main limpio.

```Java
public class Launcher {
    public static void main(String[] args)
    throws LoginException, RateLimitedException {
            Bot bot = new Bot();
            bot.run();
	}
}
```

Con esto el obtenemos un lanzador limpio del Bot.

## Plugins

Crearemos un paquete en el que almacenaremos todos los plugins, y dentro crearemos una clase abstracta que será el modelo de Plugins que todas las subclases deberán cumplir.

```Java
public abstract class AbstractPlugin {
    
}
```

El constructor pedirá tres cosas:

- `JDA bot`: El objeto JDA que comportará el propio bot del sistema.
- `Message message`: El objeto Message que capturará el mensaje enviado por el usuario.
- `MessageChannel channel`: El objeto Channel que será el canal por el que ha sido enviado dicho mensaje.

Dejando nuestro super constructor tal que así:

```Java
public abstract class AbstractPlugin {
    
    public AbstractPlugin(JDA bot, Message message, MessageChannel channel) {

    }

}
```

Reogeremos estas variables de manera estática para que puedan ser usadas por la clase entera, en lugar de objetos de clase.

```Java
public abstract class AbstractPlugin {

    protected static JDA bot;
    protected static Message message;
    protected static MessageChannel channel;
    
    public AbstractPlugin(JDA bot, Message message, MessageChannel channel) {
        AbstractPlugin.bot = bot;
        AbstractPlugin.message = message;
        AbstractPlugin.channel = channel;
    }

}
```

Ahora crearemos un método abstracto que pondrá a funcionar el plugin y que, al ser abstracto, deberá ser implementado por todas las subclases de `AbstractPlugin`.

```Java
public abstract class AbstractPlugin {

    protected static JDA bot;
    protected static Message message;
    protected static MessageChannel channel;
    
    public AbstractPlugin(JDA bot, Message message, MessageChannel channel) {
        AbstractPlugin.bot = bot;
        AbstractPlugin.message = message;
        AbstractPlugin.channel = channel;
    }

    /**
     * Método obligatorio que pondrá a funcionar el plugin
     */
    public abstract void run();

}
```

Probaremos nuestra clase abstracta con un simple plugin que apagará el bot.

```Java
public class Shutdown extends AbstractPlugin {

    // Tendremos que implementar el super constructor
    public Shutdown(JDA bot, Message message, MessageChannel channel) {
        super(bot, message, channel);
    }

    // Y el método que hará funcionar nuestro plugin

    /*
     * Método para apagar el Bot
     */
    @Override
    public void run() {
        // Aquí escribiremos las acciones que realizará el bot
        channel.sendMessage("Bye, bye~").queue();
        bot.shutdown();
    }
    
}
```

## Activación

Con esto quedaría finalizada nuestra súper clase para manejar Plugins, ahora solo queda recoger qué plugin está activando el usuario.

Volveremos a nuestra clase Bot y añadiremos el método que recoge mensajes del servidor.

```Java
@Override
public void onMessageReceived(MessageReceivedEvent event) {
    /*
     * Declaramos un prefijo,
     * a menos que queramos que el bot recoja todos los mensajes
     */
    String prefix = "test.";

    // Recogemos el contenido del mensaje
    String msg = event.getMessage().getContentRaw();

    // Comprobamos si el mensaje empieza por el prefijo
    // y el autor no sea el bot
    if(msg.startsWith(prefix) && !event.getAuthor().isBot()) {
        
    }
}
```

Recogeremos el nombre del Plugin (test.shutdown -> shutdown) y lo usaremos para inicializar nuestra clase, por lo que recurriremos a Class, Constructor y Object.

```Java
@Override
public void onMessageReceived(MessageReceivedEvent event) {
    /*
     * Declaramos un prefijo,
     * a menos que queramos que el bot recoja todos los mensajes
     */
    String prefix = "test.";

    // Recogemos el contenido del mensaje
    String msg = event.getMessage().getContentRaw();

    // Comprobamos si el mensaje empieza por el prefijo
    // y el autor no sea el bot
    if(msg.startsWith(prefix) && !event.getAuthor().isBot()) {
        // Recogemos el plugin, eliminando el prefijo y cualquier
        // cosa seguida de una espacio
        String plugin = msg.replace(prefix, "").split(" ")[0];

        // Convertimos la primera letra en mayúscula
        plugin = Character.toUpperCase(plugin.charAt(0)) + plugin.substring(1);

        // Declaramos el paquete donde estarán almacenados
        // nuestros plugins
        String package = "plugins";

        // Deberemos capturar excepciones
        try {
            // Inicializamos la clase con la información que tenemos
            // Declaramos como (?) porque es de tipo indefinido
            Class<?> pluginCls = Class.forName(package + "." + plugin);
        } catch (ClassNotFoundException e) {
            System.err.println(plugin + " plugin not found!");
        } catch (Exception e2) {
            System.err.println(e2.getMessage());
        }
    }
}
```

De la clase que acabamos de crear, seleccionaremos todos los cosntructores y obtendremos solo el primero (el único que hemos creado, realmente) para poder usarlo para instaciar el Objeto.

```Java
@Override
public void onMessageReceived(MessageReceivedEvent event) {
    /*
     * Declaramos un prefijo,
     * a menos que queramos que el bot recoja todos los mensajes
     */
    String prefix = "test.";

    // Recogemos el contenido del mensaje
    String msg = event.getMessage().getContentRaw();

    // Comprobamos si el mensaje empieza por el prefijo
    // y el autor no sea el bot
    if(msg.startsWith(prefix) && !event.getAuthor().isBot()) {
        // Recogemos el plugin, eliminando el prefijo y cualquier
        // cosa seguida de una espacio
        String plugin = msg.replace(prefix, "").split(" ")[0];

        // Convertimos la primera letra en mayúscula
        plugin = Character.toUpperCase(plugin.charAt(0)) + plugin.substring(1);

        // Declaramos el paquete donde estarán almacenados
        // nuestros plugins
        String package = "plugins";

        // Deberemos capturar excepciones
        try {
            // Inicializamos la clase con la información que tenemos
            // Declaramos como (?) porque es de tipo indefinido
            Class<?> pluginCls = Class.forName(package + "." + plugin);

            // Recogemos constructores y seleccionamos el primero
            Constructor<?> pluginCons = pluginCls.getConstructors()[0];

            // Inicializamos pasándo por parámetros lo que
            // señalamos en AbstractPlugin
            Object pluginObj = pluginCons.newInstance(bot, event.getMessage(), event.getChannel());
        } catch (ClassNotFoundException e) {
            System.err.println(plugin + " plugin not found!");
        } catch (Exception e2) {
            System.err.println(e2.getMessage());
        }
    }
}
```

Una vez creado el Objeto bastará con convertirlo en uno de tipo `AbstractPlugin` (nuestra súper clase) y ejecutar el método `run()` obligatorio para todos los plugins.

```Java
@Override
public void onMessageReceived(MessageReceivedEvent event) {
    /*
     * Declaramos un prefijo,
     * a menos que queramos que el bot recoja todos los mensajes
     */
    String prefix = "test.";

    // Recogemos el contenido del mensaje
    String msg = event.getMessage().getContentRaw();

    // Comprobamos si el mensaje empieza por el prefijo
    // y el autor no sea el bot
    if(msg.startsWith(prefix) && !event.getAuthor().isBot()) {
        // Recogemos el plugin, eliminando el prefijo y cualquier
        // cosa seguida de una espacio
        String plugin = msg.replace(prefix, "").split(" ")[0];

        // Convertimos la primera letra en mayúscula
        plugin = Character.toUpperCase(plugin.charAt(0)) + plugin.substring(1);

        // Declaramos el paquete donde estarán almacenados
        // nuestros plugins
        String package = "plugins";

        // Deberemos capturar excepciones
        try {
            // Inicializamos la clase con la información que tenemos
            // Declaramos como (?) porque es de tipo indefinido
            Class<?> pluginCls = Class.forName(package + "." + plugin);

            // Recogemos constructores y seleccionamos el primero
            Constructor<?> pluginCons = pluginCls.getConstructors()[0];

            // Inicializamos pasándo por parámetros lo que
            // señalamos en AbstractPlugin
            Object pluginObj = pluginCons.newInstance(bot, event.getMessage(), event.getChannel());

            AbstractPlugin runner = (AbstractPlugin) pluginObj;
            runner.run();
        } catch (ClassNotFoundException e) {
            System.err.println(plugin + " plugin not found!");
        } catch (Exception e2) {
            System.err.println(e2.getMessage());
        }
    }
}
```

Como añadido, podemos ejecutar un mensaje de prueba en caso de que el usuario quiera ejecutar un plugin que no existe.

Para ello bastará con modificar un poco el `catch` de `ClassNotFoundException`.

```Java
@Override
public void onMessageReceived(MessageReceivedEvent event) {
    /*
     * Declaramos un prefijo,
     * a menos que queramos que el bot recoja todos los mensajes
     */
    String prefix = "test.";

    // Recogemos el contenido del mensaje
    String msg = event.getMessage().getContentRaw();

    // Comprobamos si el mensaje empieza por el prefijo
    // y el autor no sea el bot
    if(msg.startsWith(prefix) && !event.getAuthor().isBot()) {
        // Recogemos el plugin, eliminando el prefijo y cualquier
        // cosa seguida de una espacio
        String plugin = msg.replace(prefix, "").split(" ")[0];

        // Convertimos la primera letra en mayúscula
        plugin = Character.toUpperCase(plugin.charAt(0)) + plugin.substring(1);

        // Declaramos el paquete donde estarán almacenados
        // nuestros plugins
        String package = "plugins";

        // Deberemos capturar excepciones
        try {
            // Inicializamos la clase con la información que tenemos
            // Declaramos como (?) porque es de tipo indefinido
            Class<?> pluginCls = Class.forName(package + "." + plugin);

            // Recogemos constructores y seleccionamos el primero
            Constructor<?> pluginCons = pluginCls.getConstructors()[0];

            // Inicializamos pasándo por parámetros lo que
            // señalamos en AbstractPlugin
            Object pluginObj = pluginCons.newInstance(bot, event.getMessage(), event.getChannel());

            AbstractPlugin runner = (AbstractPlugin) pluginObj;
            runner.run();
        } catch (ClassNotFoundException e) {
            System.err.println(plugin + " plugin not found!");

            // Mensaje para el usuario
            event.getChannel().sendMessage("`" + plugin + "` no está en al lista de Plugins.").queue();
        } catch (Exception e2) {
            System.err.println(e2.getMessage());
        }
    }
}
```

Con esto nuestro bot está listo, simplemente bastará comprobarlo.

![test](https://i.imgur.com/YYyDSe0.png)

## Ayudas

Puedes encontrar una versión más avanzada y completa de este mismo sistema, así como una plantilla para Bots en el siguiente repositorio:

[JavaBot](https://github.com/LeCuay/SimpleJavaBot "JavaBot by LeCuay") by LeCuay