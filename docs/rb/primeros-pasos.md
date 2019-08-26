

# Índice

* [Introducción](#introducción) - Introducción de la gema `discordrb` y preparación del entorno de trabajo.
	* [Requisitos](#requisitos) - Software necesario para empezar a utilizar `discordrb`.
	* [Instalación](#instalacion) - Instalación de la gema.
* [Primer bot](#primer-bot) - Tutorial básico para la creación de un bot.
	* [Discord Client](#discord-client) - Poniendo en línea el bot.
	* [Recibiendo Mensajes](#recibiendo-mensajes) - Recibiendo y enviando mensajes básicos.
* [Archivo de Configuración](#archivo-de-configuración) - Utilizando un archivo externo para evitar exponer información personal.
	* [config.json](#config.json) - Creando .json para almacenar información.
	* [Utilizando config.json](#utilizando-json) - Utilizando en el código la información almacenada en config.json.
* [CommandBot](#commandbot) - Utilizando CommandBot, el handler de comandos nativo de `discordrb`.
	* [Utilizando argumentos](#utilizando-argumentos) - El concepto de argumentos.
	* [Atributos de comando](#atributos-de-comando) - Herramientas de mejora para nuestros comandos.
  	* [Comando help](#comando-help) - Conociendo el comando `help` de `discordrb`.

## Introducción
[Discordrb](https://github.com/meew0/discordrb/ "Repositorio en GitHub de Discordrb") es una gema para [Ruby](https://www.ruby-lang.org/es/ "Web oficial de Ruby") hecha para facilitar el uso de [Discord API](https://discordapp.com/developers/docs/intro "Discord API Documentation").

Fue desarrollada por [meew0](https://github.com/meew0 "Perfil en GitHub de meew0")

### Requisitos
* [Ruby](###Ruby)
* [gem y Bundler](###gem-y-bundler)
* [Software para soporte de audio](###software-para-soporte-de-audio)

#### Ruby
`Ruby` es un lenguaje de programación orientado a objetos dinámico y de código abierto enfocado en la simplicidad y productividad.

Para descargarlo, diríjase al apartado de descargas en la [web oficial](https://www.ruby-lang.org/es/downloads/ "Descargar Ruby") y elija la versión estable/recomendada \(`2.6.3` en el momento que se escribió esta guía\)

#### gem y bundler
`gem` es el servicio encargado de manejar las gemas de Ruby desarrollados por la comunidad, consiste en un comando \(instalado junto con Ruby\) que nos permite instalar y mantener actualizada cualquier gema alojada en su base de datos.

Alternativamente, `bundler` es una alternativa al package manager `gem`, `bundler` es una salida del infierno de la dependencia y garantiza que las gemas que necesita estén presentes en el desarrollo, la puesta en escena y la producción.

En esta guía, utilizaremos ambos package managers, ambos funcionan de distinta manera, pero descuida, son totalmente sencillos!

Para instalar bundler, debemos utilizar el siguiente comando **(ya debemos tener Ruby instalado)**: [Bundler docs](https://bundler.io/man/bundle-install.1.html, "Documentación oficial de Bundler")
```sh
gem install bundler
```

#### Software para soporte de audio
Si planea utilizar alguna función que requiera audio (reproducir música en un canal de voz por ejemplo), necesitará instalar algunas gemas adicionales:

Nota: En Windows si desea utilizar la función de voz, su versión de Ruby debe ser 32bits (x86), de lo contrario, Opus no funcionará.

* **Software:**
  * Libsodium [Instalación Libsodium](https://github.com/meew0/discordrb/wiki/Installing-libsodium "Instalación de Libsodium por meew0")
  * Una distribución compilada de libopus [Instalación Libopus](https://github.com/meew0/discordrb/wiki/Installing-libopus "Instalación de Libopus por meew0")
  * FFmpeg
  ```console
  gem install ffmpeg
  ```

### Instalación
Para empezar a utilizar `discordrb` necesitará crear una carpeta donde estarán ubicados los archivos necesarios para el funcionamiento del bot.

* **Windows:**
Luego de esto, deberá abrir el `cmd` \(Win + R y escriba "cmd" en ejecutar\) y abrirá la ruta donde se encuentra la carpeta con el comando `cd`, ejemplo:
```console
cd desktop/bot
```

* **Linux:**
Luego de esto, deberá abrir la `terminal` \(Aplicaciones»Accesorios»Terminal\) y abrirá la ruta donde se encuentra la carpeta con el comando `cd`, ejemplo:
```console
cd desktop/bot
```

> **Consejo:** Puede escribir `cmd` en el buscador de carpetas para abrir una consola.

* **Utilizando Bundler:**
Hecho lo anterior, solo debe ejecutar el siguiente comando que creará un archivo Gemfile que contendrá las gemas necesarias del bot. El contenido que trae por defecto puede eliminarse.
```console
bundle init
```
    * Abrimos el Gemfile y colocamos la estructura, ejemplo:
    ```rb
    source "https://rubygems.org/" #Este es el source del cual se descargarán las gemas.

    gem "discordrb" #Acá se solicita la gema de discordrb.
    ```
    * Luego, instalamos las gemas del Gemfile con el siguiente comando:

    ```console
    bundle install #Esto instalará todas las gemas requeridas en el Gemfile y creará un archivo Gemfile.lock
    ```

* **Utilizando gem:**
Hecho lo anterior, solo debe ejecutar el siguiente comando:

```console
#Linux/MacOS
gem install discordrb

#Windows
gem install discordrb --platform=ruby
```

Si recibe este error al instalar la gema:
```console
Error: Error installing discordrb:
       The 'websocket-driver' native gem requires installed build tools.
```
Es porque no tiene el Devkit instalado el cual es necesario para construir extensiones nativas.

De esta manera, se empezará a instalar `discordrb` con todas sus dependencias en el directorio seleccionado.

Si no hubo ningún mensaje de error al finalizar la instalación, es hora de programar su bot.

## Iniciando
Una vez explicados los puntos básicos y teniendo todo listo, podemos empezar a usar `discordrb`, esta sección cubrirá los primeros pasos en la creación de un bot (Eventos, CommandBot).

Primero vamos a crear un nuevo archivo con extensión `.rb` (ej: `BotRuby.rb`).
>**Importante:** en el Tipo de archivo al guardar debe elegir "Todos los archivos", de otra manera, es posible que se guarde como .txt.

Antes de nada, al igual que con cualquier otra gema de `Ruby`, se precisa de la función `require 'gem'` para empezar a trabajar con ella, así que vamos a declarar una variable que contenga `discordrb`.
```ruby
require 'discordrb' #Con esto solicitamos la gema discordrb en el archivo del bot.
```
### Discord Client
La clase [`Bot`]("Bot") es el principal punto de interacción de `discordrb` con `Discord API`, primero deberá crear una nueva instancia de `Discord.Client` e iniciar sesión con el token obtenido en [Mis aplicaciones](https://discordapp.com/developers/applications/me "Discord Developers"). Con `bot.run` iniciamos el bot.
```rb
bot = Discordrb::Bot.new token: '<token>' #Declaración del cliente y token a utilizar.

bot.run
```
Luego para comprobar que todo haya salido bien, utilizaremos el evento [`ready`](https://www.rubydoc.info/gems/discordrb/Discordrb/EventContainer#ready-instance_method "EventContainer#ready-instance_method"), que es emitido en cuanto nuestro bot está en línea.
```rb
bot.ready() do |ready|
  puts "Estoy en línea!"
end
```
Luego ejecute el comando `ruby` seguido del nombre del archivo de su bot, ejemplo:
```console
ruby bot.rb
```
Si se imprime en la consola el mensaje "Estoy en línea!", todo salió bien.

### Recibiendo mensajes
Ahora que nuestro bot está en linea, comprobaremos su funcionamiento intentando que reciba y envíe mensajes, para ello precisamos del evento [`message`](https://www.rubydoc.info/github/meew0/discordrb/Discordrb/Message "Discordrb::Message"), este emitirá un [objeto](https://www.rubydoc.info/github/meew0/discordrb/Discordrb/Message "Message") con todas las propiedades del mensaje (autor, contenido, canal).
```rb
bot.message() do |msg|
	puts (msg.author.tag, msg.content);
end
```
En este ejemplo, se estaría imprimiendo en la consola el autor y contenido de cada mensaje que presencie el bot, si queremos que ignore el resto de mensajes exceptuando el que usted le indique, deberá crear una condición que compare el contenido de los mensajes.

```rb
if message.content.start_with("ping")
	message.send_message("Pong!")
end
```
En este ejemplo, `message.content` representaría el contenido del mensaje enviado, utilizando `start_with` comprobamos que el mensaje empiece por "ping", de esta manera, solo si se cumple esa condición, enviaría un mensaje que contenga "Pong".

## Archivo de Configuración
Puede que quieras enseñarle tu código a alguien o subirlo a un repositorio en GitHub, pero este contiene el token de tu bot, alguna API key o cuenta personal, para evitar exponer esta información y ya de paso poder acceder a esta más fácilmente, podemos utilizar un archivo de configuración.

### config.json
Lo primero que hará, será crear un archivo en la carpeta de nuestro bot y le colocamos el nombre `config.json`(no necesariamente debe ser ese nombre, pero la extensión sí debe ser .json).

El contenido del archivo debe ser similar al siguiente:
```json
{
	"prefix": "!",
	"token": "Token",
	"ownerID": "159158446304788481"
}
```

### Utilizando config.json
El config.json lo utilizaremos con un `read()`, `parse()` de la siguiente manera:
```rb
#También debemos instalar y solicitar json.

#Gemfile:
gem 'json'

#gem:
gem install 'json'

require 'json'
config_file = File.read('./config.json')
config = JSON.parse(config_file)
token = config['token']
config['prefix']
```
>**Importante:** el `./` para que detecte que está en un directorio y no es una gema.

Y luego solo queda implementarlo en el código
```rb
Discordrb::Bot.new token: token
```

## CommandBot
Discord.rb posee un sistema que facilita la creación de comandos, permitiendo añadir distintos parámetros como cooldown, permisos, y ahorrándote líneas de código.
Para comenzar a utilizar `CommandBot` debemos realizar pequeñas modificaciones en la definición del `cliente`.
```rb
bot = Discordrb::Commands::CommandBot.new token: '<token>', client_id: ID_BOT, prefix: '!'
```

Como verán la definición de `bot` cambió drásticamente. Ahora podemos añadirle más parámetros y condiciones que se cumplirán en los comandos que iremos agregando.

> Puede ver todos los parámetros [aquí](https://www.rubydoc.info/gems/discordrb/Discordrb/Commands/CommandBot)

Ahora definiremos un comando básico, el `ping`.
```rb
bot.command :ping do |event|
  return "Pong!"
end
```

Ahora utilizamos `!ping` y el bot debería responder `Pong!`.

#### Utilizando argumentos
Podemos definir argumentos para añadir más utilidades a nuestro bot. Estos serán útiles para crear comandos más complejos o con múltiples funciones.

```rb
bot.command :say do |event, args|
  return args
end
```

En el ejemplo de arriba definimos un nuevo comando llamado `say` el cual repetirá la primera palabra que escribas seguida del comando. Debería ser algo como esto: `!say hola`, entonces el bot retornaria `hola`.

Pero supongamos que el usuario escribe `!say hola soy lau`, el bot solo retornaría `hola` y no queremos eso, así que en donde definimos `args` agregaremos un `*` delante.
```rb
bot.command :say do |event, *args|
  return args.join(" ")
end
```

Al agregar el `*` delante de args, estamos definiendo que `args` será un `array`. Con `join(" ")` estamos uniendo el array con un espacio en blanco en un string.
Una vez explicado el funcionamiento, si utilizamos `!say Hola soy lau` debería enviarnos `Hola soy lau`.

#### Atributos de comando
Ya vimos que definir comandos no es ninguna ciencia compleja, así que ahora les añadiremos más contenido con los `atributos`. Estos atributos permitirán que nuestros comandos tengan más funciones.
```rb
bot.command :say, permission_message: "No puedes usar esto", required_permissions: [:KICK_MEMBERS], description: "Repetiré lo que digas", usage: "args" do |event, *args|
  return args.join(" ")
end
```

Como verán la definición del comando se extendió considerablemente. Permítame explicar cada atributo punto por punto.

	* `permission_message` será el mensaje de retorno cuando el usuario no tenga permisos suficientes para utilizar el comando.
	* `required_permissions` es un array que contiene los permisos necesarios para utilizar el comando.
	* `description` será el mensaje mostrado en el comando [help](#comando-help)
	* `usage` es el uso del comando mostrado en el `help`.

> **Nota:** Puedes ver todos los atributos en [CommandContainer](https://www.rubydoc.info/gems/discordrb/Discordrb/Commands/CommandContainer).

#### Comando help
Discordrb nos brinda un comando `help` nativo. No hace falta escribir ni **una sola línea de código** para comenzar a usar este comando, simplemente tipea tu `prefijo` seguido de `help`.

Muchos de los parámetros que especificaras mientras creas tus comandos serán mostrados en el comando help. Por ejemplo, el parámetro `description` mostrará una descripción breve del comando.

> **Nota:** Puedes sobreescribir este comando definiendo un nuevo comando con el nombre `help`.

> **Nota 2:** Hasta el momento no he descubierto forma de acceder a la gema para traducir el comando.
