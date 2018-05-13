# Índice 

* [Introducción](#introducción) - Introducción de la librería `discord.js` y preparación del entorno de trabajo.	
	* [Requisitos](#requisitos) - Software necesario para empezar a utilizar `discord.js`.	
	* [Instalación](#instalacion) - Instalación de la librería.	
* [Primer bot](#primer-bot) - Tutorial básico en para la creación de un bot.	
	* [Discord Client](#discord-client) - Poniendo en línea el bot.	
	* [Recibiendo Mensajes](#recibiendo-mensajes) - Recibiendo y enviando mensajes básicos.	
	* [Command Handler](#command-handler) - Añadiendo comandos y parámetros.


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
> Consejo: También puedes utilizar `Shift + Click Derecho` en la carpeta para abrir un cmd en ese directorio.  

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
La clase `Client` es el principal punto de interacción de `discord.js` con `Discord API`, primero deberá crear una nueva instancia de `Discord.Client` e iniciar sesión con el token obtenido en [My apps](https://discordapp.com/developers/applications/me "Discord Developers").
```js
const client = new Discord.Client();

client.login(TOKEN);
```
Luego para comprobar que todo haya salido bien, utilizaremos el evento `ready`, que es emitido en cuanto nuestro bot está en línea.
```js
client.on("ready", () => { 
	console.log("Conectado como " + client.user.tag); 
});
```
Si se imprime en la consola el mensaje "Conectado como `DiscordTagDeNuestroBot`", todo salió bien.
### Recibiendo mensajes
Ahora que nuestro bot está en linea, comprobaremos su funcionamiento intentando que reciba y envíe mensajes, para ello precisamos del evento `message`, este emitirá un objeto con todas las propiedades del mensaje (autor, contenido, canal).
```js
client.on('message', msg => { 
	console.log(msg.author.tag, msg.content) 
});
```
En este ejemplo, se estaría imprimiendo en la consola el autor y contenido de cada mensaje que presencie el bot, si queremos que ignore el resto de mensajes exceptuando el que usted le indique, deberá crear una condición que compare el contenido de los mensajes.
> **Importante**: el siguiente ejemplo debe ubicarse dentro del evento `message` (al igual que todo lo que veremos a continuación) y no es la manera correcta para agregar comandos, más adelante veremos como crear un command handler.
```js
if (message.content.startsWith("ping")) {
	message.channel.send("Pong!");
}
```
En el este ejemplo `message.content` representaría el contenido del mensaje enviado, utilizando `startsWith` comprobamos que el mensaje empiece por "ping", de esta manera, solo si se cumple esa condición, enviaría un mensaje que contenga "Pong".
>Otra aclaratoria rápida y muy importante, `message` no es un método para enviar mensajes, es solo el objeto emitido por el evento, el método correcto sería `send()`, que solo funciona sobre un canal de texto, en este caso `message.channel` que sería el canal en el que fue enviado el mensaje.

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

¿Suena complicado?, permítame decir que no lo es, vamos a declarar otras dos variables y procederé a explicar el funcionamiento de estas.
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
	* `args.shift()` separaría el comando del resto del mensaje (`shift()` remueve el primer objeto de un array), de esta manera `args` solo quedaría como el resto del contenido del mensaje y lo podremos utilizar para definir parámetros adicionales en nuestros comandos.
	*  `toLowerCase()` haría que todo el comando estuviera en minúsculas, así en caso de que nos equivoquemos escribiendo el comando y pongamos algo como `/Saludo`, funcionaría igual.
*  `content` sería similar a `args`, solo que en lugar de tener un array, sería un string.
	* `join(" ")` es la función que se encarga de unir todo los elementos del array con espacios en un string.

#### Comandos
Habiendo entendido todo, prosigamos a hacer un comando.
```js
if (command === "ping") {
	message.channel.send("Pong!");
}
```
Más simple eh?, de esta manera no tendrán los problemas de `startsWith()` (anteriormente, sí poníamos cosas como "pingasdasdlkh", el mensaje se enviaba igual ya que la condición detectaba que empieza por "ping".

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
	message.channel.send("Hola, mi nombre es " + args[0] + ", tengo " + args[1] + " años y actualmente vivo en " + args[2]);
}
```
Si utilizamos el comando de la siguiente manera: `/presentacion Daniel 22 Venezuela`, el bot enviaría "Hola, mi nombre es Daniel, tengo 22 años y actualmente vivo en Venezuela.

Bastante útil, pero ahora te toca a ti crear tus propios comandos.

#### Notas adicionales
Puedes crear tus propios mensajes de error si tu comando no encuentra algo que especificaste, como los argumentos, un ejemplo que puedes añadir al comienzo de un comando:
```js
if (!args) return message.channel.send("Introduzca algunos parámetros")
```
También recomiendo ignorar los mensajes de los bots, si interactúan entre ellos podría ocurrir un desastre (~~temo que puedan querer apoderase de nuestors servidores~~), para evitarlo solo añade la siguiente condición al comienzo del evento `message`:
```js
if (message.author.bot) return;
```

