

# Índice

* [Introducción](#introducción) - Introducción de la gema `discordrb` y preparación del entorno de trabajo.
	* [Requisitos](#requisitos) - Software necesario para empezar a utilizar `discordrb`.
	* [Instalación](#instalacion) - Instalación de la gema.
* [Primer bot](#primer-bot) - Tutorial básico para la creación de un bot.
	* [Discord Client](#discord-client) - Poniendo en línea el bot.
	* [Recibiendo Mensajes](#recibiendo-mensajes) - Recibiendo y enviando mensajes básicos.
	* [Command Handler](#command-handler) - Añadiendo comandos y parámetros.
* [Archivo de Configuración](#archivo-de-configuración) - Utilizando un archivo externo para evitar exponer información personal.
	* [config.json](#config.json) - Creando .json para almacenar información.
	* [Utilizando config.json](#utilizando-json) - Utilizando en el código la información almacenada en config.json.
* [Conceptos](#conceptos) - Explicación a fondo de algunas funciones de `discordrb`.
	* [Eventos](#eventos) - Entendiendo el punto más importante de `discordrb`.
	* [Colecciones](#colecciones) - Objetos hechos para almacenar varios elementos más fácilmente.

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

> **Consejo:** También puedes utilizar `Shift + Click Derecho` en la carpeta para abrir un cmd en ese directorio.  

* **Utilizando Bundler:**
Hecho lo anterior, solo debe ejecutar los siguientes comandos:
```console
bundle init #Esto creará un archivo Gemfile que contendrá las gemas necesarias del bot. El contenido que trae por defecto el Gemfile puede eliminarse.
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
Es porque no tienes el Devkit instalado el cual es necesario para construir extensiones nativas.

De esta manera, se empezará a instalar `discordrb` con todas sus dependencias en el directorio seleccionado.

Si no hubo ningún mensaje de error al finalizar la instalación, es hora de programar un bot.

## Primer bot
Una vez explicados los puntos básicos y teniendo todo listo, podemos empezar a usar `discordrb`, esta sección cubrirá los primeros pasos en la creación de un bot (Respuestas, Uso de JSON y Argumentos).

Primero vamos a crear un nuevo archivo con extensión `.rb` (ej: `BotRuby.rb`).
>**Importante:** en el Tipo de archivo al guardar debe elegir "Todos los archivos", de otra manera, es posible que se guarde como .txt.

Antes de nada, al igual que con cualquier otra gema de `Ruby`, se precisa de la función `require 'gem'` para empezar a trabajar con ella, así que vamos a declarar una variable que contenga `discordrb`.
```rb
require 'discordrb' #Con esto solicitamos la gema discordrb en el archivo del bot.
```
### Discord Client
La clase [`Bot`]("Bot") es el principal punto de interacción de `discordrb` con `Discord API`, primero deberá crear una nueva instancia de `Discord.Client` e iniciar sesión con el token obtenido en [My apps](https://discordapp.com/developers/applications/me "Discord Developers").
```rb
bot = Discordrb::Bot.new token: '<token>' #Declaración del cliente y token a utilizar.

bot.run
```
Luego para comprobar que todo haya salido bien, utilizaremos el evento [`ready`](https://www.rubydoc.info/gems/discordrb/Discordrb/EventContainer#ready-instance_method "EventContainer#ready-instance_method"), que es emitido en cuanto nuestro bot está en línea.
```rb
bot.ready() do |ready|
  puts "I'm online!"
end
```
Luego ejecute el comando `ruby` seguido del nombre del archivo de su bot, ejemplo:
```console
ruby bot.rb
```
Si se imprime en la consola el mensaje "I'm online!", todo salió bien.

### Recibiendo mensajes
Ahora que nuestro bot está en linea, comprobaremos su funcionamiento intentando que reciba y envíe mensajes, para ello precisamos del evento [`message`](https://www.rubydoc.info/github/meew0/discordrb/Discordrb/Message "Discordrb::Message"), este emitirá un [objeto](https://www.rubydoc.info/github/meew0/discordrb/Discordrb/Message "Message") con todas las propiedades del mensaje (autor, contenido, canal).
```rb
bot.message() do |msg|
	puts (msg.author.tag, msg.content);
end
```
En este ejemplo, se estaría imprimiendo en la consola el autor y contenido de cada mensaje que presencie el bot, si queremos que ignore el resto de mensajes exceptuando el que usted le indique, deberá crear una condición que compare el contenido de los mensajes.
> **Importante:** el siguiente ejemplo debe ubicarse dentro del evento `message` (al igual que todo lo que veremos a continuación) y no es la manera correcta para agregar comandos, más adelante veremos como crear un command handler.

```rb
if message.content.start_with("ping")
	message.send_message("Pong!")
end
```
En este ejemplo, `message.content` representaría el contenido del mensaje enviado, utilizando `start_with` comprobamos que el mensaje empiece por "ping", de esta manera, solo si se cumple esa condición, enviaría un mensaje que contenga "Pong".

>**Otra aclaratoria rápida y muy importante:** `message` no es un método para enviar mensajes, es solo el objeto emitido por el evento, el método correcto sería `respond()` o `send_message()`, que solo funciona sobre un canal de texto, en este caso `message.channel` que sería el canal en el que fue enviado el mensaje.

### Command Handler
Es hora de darle forma a nuestro bot y empezar a añadir comandos. En el ejemplo de la sección anterior pudimos hacer que nuestro bot reaccione a los mensajes que le indiquemos, pero esto no es suficiente, se precisa de algo que filtre los mensajes de una manera más cómoda y tome los parámetros adicionales que le indique el usuario (argumentos).

Primero que nada elija un prefijo, algo único que diferencie los comandos de su bot y nos ocuparemos de que solo responda a los comandos que comiencen por este.
```rb
Prefix = "/";

if message.content.start_with(prefix)
  #Código
end
```
> No usen `/` de prefijo, es solo un ejemplo.

#### Argumentos
Una vez hecho lo anterior, para terminar el Command Handler debemos hacer dos cosas:
* Eliminar el prefijo dejando solo el comando.
* Tomar el resto del contenido del mensaje como argumentos.

¿Suena complicado?, permítanme decir que no lo es, vamos a declarar otras tres variables y procederé a explicar el funcionamiento de estas.
```rb
Args = message.content.slice(2, prefix.length).split(' ').strip
Command = message.content.slice(2, prefix.length).split(' ')[0].downcase
Args -= [Command]
content = Args.join(" ")
```
Supongamos que tenemos el siguiente comando: `/saludo Me llamo blood`

* En la primera variable (`Args`), obviamente lo primero que hacemos es tomar el contenido del mensaje.
	* Con `slice()` estamos cortando del mensaje nuestro prefijo, si nuestro prefijo es `/`, un prefijo de solo un caracter, `prefix.length` solo sería `1`, por lo tanto el comando pasaría a ser solo `saludo`
	* `strip` elimina todos los espacios adicionales que puedan haber antes y después del mensaje.
	* `split(' ')` separaría el mensaje por sus espacios dejando solo un array, de esta manera nos quedaría `["saludo", "Me", "llamo", "Nakido"]`
    * `Command` sería lo que usaremos luego para agregar comandos.
	* `downcase` haría que todo el comando estuviera en minúsculas, así en caso de que nos equivoquemos escribiendo el comando y pongamos algo como `/Saludo`, funcionaría igual.
    * `content` sería similar a `args`, solo que en lugar de tener un array, sería un string.
    * `join(" ")` es la función que se encarga de unir todos los elementos del array con espacios en un string.

#### Comandos
Habiendo entendido todo, prosigamos a hacer un comando.
```rb
if Command.eql? "ping" # eql? significa igual a, si vienes de otros lenguajes, === no tiene el mismo significado en Ruby.
	message.channel.send_message("Pong!")
end
```
Más simple eh?, de esta manera no tendrán los problemas de `start_with()` (anteriormente, sí poníamos cosas como "pingdasdlkh", el mensaje se enviaba igual ya que la condición detectaba que empieza por "ping").

Ahora aprovechemos las otras dos variables y hagamos algo más interesante.
```rb
if Command.eql? "say"
	message.channel.send_message(content);
end
```
Con este comando, si utilizáramos `/say Soy el mejor`, el bot enviaría un mensaje diciendo "Soy el mejor".

Ahora intentemos algo más complicado.
```rb
if Command.eql? "presentacion"
	nombre = Args[0]
	edad = Args[1]
	pais = Args[2]
	message.channel.send_message("Hola, mi nombre es " + nombre + ", tengo " + edad + " años y actualmente vivo en " + pais)
end
```
Si utilizamos el comando de la siguiente manera: `/presentacion Blood 70 Perú`, el bot enviaría "Hola, mi nombre es Blood, tengo 70 años y actualmente vivo en Perú".

Bastante útil, pero ahora te toca a ti crear tus propios comandos.

#### Notas adicionales
Puedes crear tus propios mensajes de error si tu comando no encuentra algo que especificaste, como los argumentos, un ejemplo que puedes añadir al comienzo de un comando para que el usuario deba escribirlo:
```rb
def SinArgs
    args = Args
    if !args
      return message.channel.send("Introduzca algunos parámetros")
    end
end
```
También recomiendo ignorar los mensajes de los bots, si interactúan entre ellos podría ocurrir un desastre (~~temo que puedan querer apoderase de nuestros servidores~~), para evitarlo solo añada la siguiente condición al comienzo del evento `message`:
```rb
def MensajeBot
    a = message.from_bot?
    if a == true
        return
    end
end
```

Si queremos tomar una mención, iremos al objeto que las contiene `message.mentions.users` y agarraremos la primera con `first`
```rb
console.log(message.mentions.users.first.tag)
```

## Archivo de Configuración
Puede que quieras enseñarle tu código a alguien o subirlo a un repositorio en GitHub, pero este contiene el token de tu bot, alguna API key o cuenta personal, para evitar exponer esta información y ya de paso poder acceder a esta más fácilmente, podemos utilizar un archivo de configuración.

### config.json
Lo primero que hará, será crear un archivo en la carpeta de nuestro bot y le colocamos el nombre `config.json`(no necesariamente debe ser ese nombre, pero la extensión sí debe ser .json).

El contenido del archivo debe ser similar al siguiente:
```json
{
	"prefix": "/",
	"token": "MzI2ODA5NA6hE5h75NzByNjQx.DAkbDQ.kF7Jf66X2z4KR31yRQ58PYvy4Y",
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

## Conceptos
Puntos de `discordrb` que pueden resultar más complicados de entender y/o dominar en su totalidad.

### Eventos
Los eventos son el principal punto de interacción con nuestro bot, cada vez que "ocurre algo" en Discord mientras nuestro bot está presente (Como enviar un mensaje, crear un canal, etc), `discordrb` emitirá un evento.

Para controlar estos eventos, utilizaremos [`Bot`](https://www.rubydoc.info/github/meew0/discordrb/Discordrb/Bot "Bot").

La síntaxis básica de cada evento sería:
```rb
bot.evento(parámetros) do |evento|
	#Código a ejecutarse cuando se emita el evento
end
```
> Obviamente, si quieres usar esos parámetros debes utilizarlos dentro del evento, si la consola te da un error diciendo `undefined local variable or method 'message'` ya sabes porqué es.

Al ejecutarse un evento, los parámetros pasarían a ser el objeto que emita el evento, si por ejemplo, fuese el ya visto evento [`message`](https://www.rubydoc.info/gems/discordrb/Discordrb/Message), este objeto contendría todo los datos del mensaje que haya ejectuado el evento (Contenido, ID, Autor, Canal).

Un ejemplo con el evento [`message_edit`](https://www.rubydoc.info/gems/discordrb/Discordrb/EventContainer#message_edit-instance_method "EventContainer#message_edit") (Emitido cuando se edita un mensaje o se añade un embed):
```rb
bot.message_edit(:id, :in) do |edit|
	canal = [:in]
    def IgnorarMensajes
        c_ignorar = canal
	    if c_ignore != "198291975663779842" #Ignora los mensajes de otros canales
          return
        end
    end
	canal.send_message(saved_message.content, message.content)
end
```
>**Importante:** Los eventos son emitido en TODOS los servidores en que esté tu bot, por eso la tercera línea que ignora todos los canales menos el indicado.

Para más información y una lista con todos los eventos, diríganse al apartado **EventEmitter** en la documentación. ["EventEmitter"]().
#### Evento `ready`
```rb
bot.game = ("Dando amor en " + bot.servers.size + "servidores")

bot.ready() do |ready|
	bot.game = ("Dando amor en " + bot.servers.size + "servidores")
end
```
¿Cual es la diferencia entre el game que está dentro del evento `ready` y el que está fuera?

El de fuera nos dará `undefined`.

Esto se debe a que al momento de ejecutar el código, `bot` no está disponible debido a que demora un poco en cargar toda la información del bot (servidores, usuarios, etc), por lo tanto cualquier cosa que quieras ejecutar al encender el bot, debe estar dentro del evento `ready`.

### Colecciones
Cuando una propiedad posee varios objetos similares (como canales), son almacenados de una manera diferente para poder acceder a ellos más fácilmente, estas son las colecciones, un ejemplo de una colección sería `guild.channels`, esta contendría todos los canales de un servidor.

Las colecciones utilizan la misma estructura de los objetos `Map()` ya implementados por defecto en JavaScript pero con algunas funciones adicionales que nos permiten trabajar con ellas de una manera más simple y amplia que, lo que podría ser un array.

> Estaré explicando todo dentro del evento `message` con el fin de hacer todo más fácil de entender.

#### Convirtiendo en un array
¿No te gusta la estructura de las colecciones?, no pasa nada, existe el método `.array()` que nos devolvería un nuevo array con todo lo que contiene una colección, ejemplo:
```rb
canal = message.server.channels.array()[0]
nombre = canal.name
```
`nombre` devolvería el nombre del primer canal (en cuanto a posición) del servidor, aunque sí queremos el primer canal, existe un método más simple.

#### Primero y último
Están los métodos `first` y `last` para las colecciones, permitiéndonos acceder más fácilmente al primer y último elemento de la colección.
```rb
canales = message.server.channels
primero = canales.first
ultimo = canales.last
```
Incluso podemos incluir un parámetro para tomar varios elementos.
```rb
canales.first(2)
```
Tomaría los primeros dos, no hace falta explicar más.

#### Mapeo
`map()` es uno de los métodos más útiles para colecciones, es igual que la función [`Array.map()`](https://www.rubyguides.com/2018/10/ruby-map-method/), devuelve un array basado en la función indicada.

Supongamos que queremos todos los nombres de los usuarios de un servidor, haríamos lo siguiente:
```rb
usuarios = message.server.members
nombres = usuarios.map { |u| u.display_name }
```
También hay que tener en cuenta que la función dentro del `map()` se ejecuta por cada elemento y puede ser más de una.
```rb
usuarios.map { |u|
	message.channel.send_message(u.display_name)
	message.channel.send_message(u.id);
}
```
Esto enviaría el id y nombre de todos los usuarios de un servidor, uno por mensaje ya que se está ejecutando por cada usuario.

#### Filtrar
Es posible filtrar los elementos bajo la condición indicada utilizando `filter()` (También conocido en Ruby como `select()`).

En lugar de un array como `map()`, este devuelve una nueva colección.
```rb
message.server.members.filter{ |g| g.nickname }
```
Esto sería una colección con todos los usuarios que tienen nick, ahora intentemos utilizar `map()` ya que sí, se pueden utilizar entre sí mientras se obtenga otra colección.
```rb
message.guild.members.filterg { |g| g.nickname }.map { |u| message.channel.send_message(u.id) }
```

#### Búsqueda
Ahora los dos métodos que nos permiten buscar algo específico entre colecciones: `get()` y `find()`.

`get()` es un método para `map()`, pero podemos utilizarlo en las colecciones para buscar un elemento específico, en este caso, por id.
```rb
bot.users.get("163156330721443840")
```
Con esto obtendriamos la colección del usuario que posea esa id.

Luego tenemos `find()` que podemos utilizarlo para buscar un valor entre todos los elementos de la colección, como el nombre de un canal o usuario.
```rb
message.server.channels.find("name", "Blood")
```
Esta función utiliza dos parámetros, el primero es la propiedad que buscamos, y el segundo lo que debería contener.

#### Otros métodos
`.concat()` funciona para combinar una colección con otra/s
```rb
servidor = message.server;
una_coleccion = servidor.channels
otra_coleccion = servidor.members
ultima_coleccion = servidor.roles

una_coleccion.concat(otra_coleccion, ultima_coleccion)
```

`.find_all()` similar a `.find()` solo que este devuelve todos los elementos que coincidan con la búsqueda en lugar de solo uno.

`.rand()` devolvería un elemento aleatorio de la colección (Para números aleatorios se debe utilizar un [`Rango`](https://ruby-doc.org/core-2.2.0/Range.html)).

Para ver todos los métodos y más información, entra en la documentación de [Discordrb](https://www.rubydoc.info/gems/discordrb/index) y [Ruby Doc](https://ruby-doc.org/core-2.2.0 "Ruby Docs").
