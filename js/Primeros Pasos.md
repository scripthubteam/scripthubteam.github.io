# Índice 

* [Introducción](#introducción) - Introducción de la librería `discord.js` y preparación del entorno de trabajo.	
	* [Requisitos](#requisitos) - Software necesario para empezar a utilizar `discord.js`.	
	* [Instalación](#instalacion) - Instalación de la librería.	
* [Primer bot](#primer-bot) - Tutorial básico en para la creación de un bot.	
	* [Discord Client](#discord-client) - Poniendo en línea el bot.	
	* [Recibiendo Mensajes](#recibiendo-mensajes) - Recibiendo y enviando mensajes básicos.	
	* [Command Handler](#command-handler) - Añadiendo comandos y parámetros.
* [Archivo de Configuración](#archivo-de-configuración) - Utilizando un archivo externo para evitar exponer información personal.
	* [config.json](#config.json) - Creando .json para almacenar información.
	* [Utilizando config.json](#command-handler) - Utilizando en el código la información almacenada en config.json.
* [Conceptos](#conceptos) - Explicación a fondo de algunas funciones de `discord.js`.
	* [Eventos](#eventos) - Entendiendo el punto más importante de `discord.js`


## Introducción
[Discord.js](https://github.com/discordjs/discord.js/ "Repositorio en GitHub de Discord.js") es una librería para [node.js](https://nodejs.org/ "Web oficial de Node.js") hecha para facilitar el uso de [Discord API](https://discordapp.com/developers/docs/intro "Discord API Documentation"). 

Fue desarrollada principalmente por [hydrabolt](https://github.com/hydrabolt "Perfil en GitHub de hydrabolt") y actualmente es la librería más descargada en [npm](https://www.npmjs.com "Web oficial de npm") para la creación de bots en Discord.

En el momento en que se escribe está guía, su última versión es `v11.3` y será con la que estaremos trabajando. Cuenta con soporte para ES8 por lo que es posible utilizar `async/await` ~~aunque estamos acostumbrados a las promesas~~.

### Requisitos
* [Node.js](###node.js)
* [Software para soporte de audio](###software-para-soporte-de-audio)

#### Node.js
`Node.js` es un entorno de ejecución, en este es posible ejecutar código escrito en `JavaScript` desde un servidor en lugar de un navegador.

Para descargarlo, diríjase al apartado de descargas en la [web oficial](https://nodejs.org/en/download/ "Descargar node.js") y elija la versión estable/recomendada \(`8.11.1` en el momento que se escribió esta guía\)

##### npm
[`NPM`](https://www.npmjs.com "Web oficial de npm") es el servicio encargado de manejar los módulos de Node.js desarrollados por la comunidad, consiste en un comando \(instalado junto con node\) que nos permite instalar y mantener actualizado cualquier módulo alojado en su base de datos.

#### Software para soporte de audio
Si planea utilizar alguna función que requiera audio (reproducir música en un canal de voz por ejemplo), necesitará instalar algunos módulos adicionales:

* **En PowerShell con permisos de administrador:**
  * `npm i -g --production windows-build-tools`

* **En la carpeta que contendrá el bot:**
  * `npm i ffmpeg-binaries`
  * `npm i node-opus` (En caso de que ocurra algún error, intente con `npm i opusscript`)

### Instalación 
Para empezar a utilizar `discord.js` necesitará crear una carpeta donde estarán ubicados los archivos necesarios para el funcionamiento del bot (o utilizar la carpeta en que haya instalado Opus y ffmpeg).

Luego de esto, deberá abrir el `cmd` \(Win + R y escriba "cmd" en ejecutar\) y abrirá la ruta donde se encuentra la carpeta con el comando `cd`, ejemplo:
```console
cd desktop/bot
```
> **Consejo:** También puedes utilizar `Shift + Click Derecho` en la carpeta para abrir un cmd en ese directorio.  

Hecho lo anterior, solo debe ejecutar el siguiente comando: `npm i discord.js`.
De esta manera, se empezará a instalar `discord.js` con todas sus dependencias en el directorio seleccionado.

Si no hubo ningún mensaje de error al finalizar la instalación, es hora de programar un bot.

## Primer bot
Una vez explicados los puntos básicos y teniendo todo listo, podemos empezar a usar `discord.js`, esta sección cubrirá los primeros pasos en la creación de un bot (Respuestas, Command Handler y Argumentos).

Antes de nada, al igual que con cualquier otro módulo de `Node`, se precisa de la función `require()`para empezar a trabajar con él, así que vamos a declarar una variable que contenga `discord.js`
```js
const Discord = require("discord.js");
```
### Discord Client
La clase [`Client`](https://discord.js.org/#/docs/main/stable/class/Client "Client") es el principal punto de interacción de `discord.js` con `Discord API`, primero deberá crear una nueva instancia de `Discord.Client` e iniciar sesión con el token obtenido en [My apps](https://discordapp.com/developers/applications/me "Discord Developers").
```js
const client = new Discord.Client();

client.login("TOKEN");
```
Luego para comprobar que todo haya salido bien, utilizaremos el evento [`ready`](https://discord.js.org/#/docs/main/stable/class/Client?scrollTo=e-ready "Client#ready"), que es emitido en cuanto nuestro bot está en línea.
```js
client.on("ready", () => { 
	console.log("Conectado como " + client.user.tag); 
});
```
Luego ejecute el comando `node` seguido del nombre del archivo de su bot, ejemplo: 
```console
node bot.js
```
Si se imprime en la consola el mensaje "Conectado como `DiscordTagDeNuestroBot`", todo salió bien.
### Recibiendo mensajes
Ahora que nuestro bot está en linea, comprobaremos su funcionamiento intentando que reciba y envíe mensajes, para ello precisamos del evento [`message`](https://discord.js.org/#/docs/main/stable/class/Client?scrollTo=e-message "Client#message"), este emitirá un [objeto](https://discord.js.org/#/docs/main/stable/class/Message "Message") con todas las propiedades del mensaje (autor, contenido, canal).
```js
client.on('message', message => { 
	console.log(messagr.author.tag, message.content) 
});
```
En este ejemplo, se estaría imprimiendo en la consola el autor y contenido de cada mensaje que presencie el bot, si queremos que ignore el resto de mensajes exceptuando el que usted le indique, deberá crear una condición que compare el contenido de los mensajes.
> **Importante:** el siguiente ejemplo debe ubicarse dentro del evento `message` (al igual que todo lo que veremos a continuación) y no es la manera correcta para agregar comandos, más adelante veremos como crear un command handler.

```js
if (message.content.startsWith("ping")) {
	message.channel.send("Pong!");
}
```
En este ejemplo, `message.content` representaría el contenido del mensaje enviado, utilizando `startsWith` comprobamos que el mensaje empiece por "ping", de esta manera, solo si se cumple esa condición, enviaría un mensaje que contenga "Pong".

>**Otra aclaratoria rápida y muy importante:** `message` no es un método para enviar mensajes, es solo el objeto emitido por el evento, el método correcto sería `send()`, que solo funciona sobre un canal de texto, en este caso `message.channel` que sería el canal en el que fue enviado el mensaje.

### Command Handler
Es hora de darle forma a nuestro bot y empezar a añadir comandos. En el ejemplo de la sección anterior pudimos hacer que nuestro bot reaccione a los mensajes que le indiquemos, pero esto no es suficiente, se precisa de algo que filtre los mensajes de una manera más cómoda y tome los parámetros adicionales que le indique el usuario (argumentos).

Primero que nada elija un prefijo, algo único que diferencie los comandos de su bot y nos ocuparemos de que solo responda a los comandos que comiencen por este.
```js
const prefix = "/";

if (!message.content.startsWith(prefix)) return;
```
> No usen `/` de prefijo, es solo un ejemplo.

#### Argumentos
Una vez hecho lo anterior, para terminar el Command Handler debemos hacer dos cosas:
* Eliminar el prefijo dejando solo el comando.
* Tomar el resto del contenido del mensaje como argumentos.

¿Suena complicado?, permítanme decir que no lo es, vamos a declarar otras tres variables y procederé a explicar el funcionamiento de estas.
```js
const args = message.content.slice(prefix.length).trim().split(/ +/g);
const command = args.shift().toLowerCase();
const content = args.join(" ");
```
Supongamos que tenemos el siguiente comando: `/saludo Me llamo Nakido`

* En la primera variable (`args`), obviamente lo primero que hacemos es tomar el contenido del mensaje. 
	* Con `slice()` estamos cortando del mensaje nuestro prefijo, si nuestro prefijo es `/`, un prefijo de solo un caracter, `prefix.length` solo sería `1`, por lo tanto el comando pasaría a ser solo `saludo`
	* `trim()` elimina todos los espacios adicionales que puedan haber antes y después del mensaje.
	* `split(/ +/g)` separaría el mensaje por sus espacios dejando solo un array (utilizamos RegExp en lugar de solo un espacio en caso de que haya un espacio adicional entre palabras, error muy común en los que acostumbramos a usar Discord en celular), de esta manera nos quedaría `["saludo", "Me", "llamo", Nakido]`
* `command` sería lo que usaremos luego para agregar comandos.
	* `args.shift()` separaría el comando del resto del mensaje (`shift()` remueve el primer objeto de un array), de esta manera `args` solo quedaría como el resto del contenido del mensaje (`["Me", "llamo", Nakido]`) y lo podremos utilizar para definir parámetros adicionales en nuestros comandos.
	*  `toLowerCase()` haría que todo el comando estuviera en minúsculas, así en caso de que nos equivoquemos escribiendo el comando y pongamos algo como `/Saludo`, funcionaría igual.
*  `content` sería similar a `args`, solo que en lugar de tener un array, sería un string.
	* `join(" ")` es la función que se encarga de unir todos los elementos del array con espacios en un string.

#### Comandos
Habiendo entendido todo, prosigamos a hacer un comando.
```js
if (command === "ping") {
	message.channel.send("Pong!");
}
```
Más simple eh?, de esta manera no tendrán los problemas de `startsWith()` (anteriormente, sí poníamos cosas como "pingdasdlkh", el mensaje se enviaba igual ya que la condición detectaba que empieza por "ping".

Ahora aprovechemos las otras dos variables y hagamos algo más interesante.
```js
if (command === "say") {
	message.channel.send(content);
}
```
Con este comando, si utilizáramos `/say Soy el mejor`, el bot enviaría un mensaje diciendo "Soy el mejor".

Ahora intentemos algo más complicado.
```js
if (command === "presentacion") {
	const nombre = args[0];
	const edad = args[1];
	const pais = args[2];
		message.channel.send("Hola, mi nombre es " + nombre + ", tengo " + edad + " años y actualmente vivo en " + pais);
}
// Desestructuración de arrays en ES6
if (command === "presentacion") {
	const [nombre, edad, pais] = args;
		message.channel.send("Hola, mi nombre es " + nombre + ", tengo " + edad + " años y actualmente vivo en " + pais);
}
```
Si utilizamos el comando de la siguiente manera: `/presentacion Daniel 22 Venezuela`, el bot enviaría "Hola, mi nombre es Daniel, tengo 22 años y actualmente vivo en Venezuela".

Bastante útil, pero ahora te toca a ti crear tus propios comandos.

#### Notas adicionales
Puedes crear tus propios mensajes de error si tu comando no encuentra algo que especificaste, como los argumentos, un ejemplo que puedes añadir al comienzo de un comando para que el usuario deba escribirlo:
```js
if (!args) return message.channel.send("Introduzca algunos parámetros")
```
También recomiendo ignorar los mensajes de los bots, si interactúan entre ellos podría ocurrir un desastre (~~temo que puedan querer apoderase de nuestros servidores~~), para evitarlo solo añada la siguiente condición al comienzo del evento `message`:
```js
if (message.author.bot) return;
```
Si queremos tomar una mención, iremos al objeto que las contiene `message.mentions.users` y agarraremos la primera con `first()`
```js
console.log(message.mentions.users.first().tag)
```

## Archivo de Configuración
Puede que quieras enseñarle tu código a alguien o subirlo a un repositorio en GitHub, pero este contiene el token de tu bot, alguna API key o cuenta personal, para evitar exponer esta información y ya de paso poder acceder a esta más fácilmente, podemos utilizar un archivo de configuración.

### config.json
Lo primero que hará, será crear un archivo en la carpeta de nuestro bot y le colocamos el nombre `config.json`(no necesariamente debe ser ese nombre, pero la extensión sí debe ser .json).
>**Importante:** en el Tipo de archivo al guardar debe elegir "Todos los archivos", de otra manera, es posible que se guarde como .txt.

El contenido del archivo debe ser similar al siguiente:
```json
{
	"prefix": "/",
	"token": "MzI2ODA5NA6hE5h75NzByNjQx.DAkbDQ.kF7Jf66X2z4KR31yRQ58PYvy4Y",
	"ownerID": "159158446304788481"
}
```
Posteriormente, seguramente necesiten añadir más datos, mi archivo de configuración es mas o menos así:
```json
{
  "token":"NDQxMjgzMDUxMTM0Mzg2gkH_eyX1joM",
  "prefix": ".",
  "APIkeys": {
    "yandex": "022T04676edde956323dd922",
    "osu": "413a4c89d96f5e4453e466"
  },
  "mal": {
    "user": "Nakido", 
    "pass": "passchida"
  }
}
```

### Utilizando config.json
El config.json lo utilizaremos con un `require()` de la siguiente manera:
```js
const { prefix, token } = require("./config.json");
```
>**Importante:** el `./` para que detecte que está en un directorio y no es un módulo.

Y luego solo queda implementarlo en el código
```js
client.login(config.token);
```

## Conceptos
Puntos de `discord.js` que pueden resultar más complicados de entender y/o dominar en su totalidad.

### Eventos
Los eventos son el principal punto de interacción con nuestro bot, cada vez que "ocurre algo" en Discord mientras nuestro bot está presente (Como enviar un mensaje, crear un canal, etc), `discord.js` emitirá un evento.

Para controlar estos eventos, utilizaremos [`Client`](https://discord.js.org/#/docs/main/stable/class/Client "Client").

La síntaxis básica de cada evento sería:
```js
client.on("NombreDelEvento", (parámetros) => {
	//Código a ejecutarse cuando se emita el evento
});
```
> Obviamente, si quieres usar esos parámetros debes utilizarlos dentro del evento, si la consola te da un error diciendo `message is not defined` ya sabes porqué es.

Al ejecutarse un evento, los parámetros pasarían a ser el objeto que emita el evento, si por ejemplo, fuese el ya visto evento [`message`](https://discord.js.org/#/docs/main/stable/class/Client?scrollTo=e-message "Client#message"), este objeto contendría todo los datos del mensaje que haya ejectuado el evento (Contenido, ID, Autor, Canal).

Un ejemplo con el evento [`messageUpdate`](https://discord.js.org/#/docs/main/stable/class/Client?scrollTo=e-message "Client#messageUpdate") (Emitido cuando se edita un mensaje o se añade un embed):
```js
client.on("messageUpdate", (oldmsg, newmsg) => {
	let canal = oldmsg.channel; 
	if(canal !== "198291975663779842") return; //Ignora los mensajes de otros canales
		canal.send(oldmsg.content, newmsg.content)
})
```
>**Importante:** Los eventos son emitido en TODOS los servidores en que esté tu bot, por eso la tercera línea que ignora todos los canales menos el indicado.

Para más información y una lista con todos los eventos, diríganse al apartado **Events** en la documentación de la clase [`Client`](https://discord.js.org/#/docs/main/stable/class/Client).
#### Evento `ready`
```js
client.user.setActivity("Dando amor en " + client.guilds.size + "servidores")

client.on("ready", () => {
  client.user.setActivity("Dando amor en " + client.guilds.size + "servidores");
});
```
¿Cual es la diferencia entre el setActivity que está dentro del evento `ready` y el que está fuera?

El de fuera nos dará `undefined`.

Esto se debe a que al momento de ejecutar el código, `client` no está disponible debido a que demora un poco en cargar toda la información del bot (servidores, usuarios, etc), por lo tanto cualquier cosa que quieras ejecutar al encender el bot, debe estar dentro del evento `ready`.

#### Probando eventos
Supongamos que estamos haciendo un sistema de bienvenidas y queremos probarlo, lo primero que se te vendrá a la cabeza seguramente sea entra y salir constantemente con una multicuenta, pero esto es bastante molesto y pesado, en su lugar podemos "simular" un evento con `emit()`.
```js
client.emit("guildMemberAdd", message.member);
```
Como puedes observar, es una función, el primer parámetro debe ser el nombre del evento y el segundo, sería el objeto que emitiría el evento, pero en este caso debemos declararlo nosotros, en el ejemplo anterior, estoy tomando al autor del mensaje como ese usuario que "entró".
