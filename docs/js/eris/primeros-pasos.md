

# Índice

* [Introducción](#introducción) - Introducción a `eris` y preparación del entorno de trabajo.
  * [Requisitos](#requisitos) - Software necesario para utilizar `eris`.
  * [Instalación](#instalación) - Instalación del paquete.
* [Primer bot](#primer-bot) - Tutorial introductorio para desarrollar tu primer bot.
  * [Discord Client](#discord-client) - Poniendo en línea nuestro bot.
  * [Recibiendo mensajes](#recibiendo-mensaje) - Recibiendo y enviando mensajes básicos.
  * [Comandos](#comandos) - Creando comandos básicos con argumentos.

## Introducción

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
Para empezar a utilizar `eris` necesitará crear una carpeta donde estarán ubicados los archivos necesarios para el funcionamiento del bot.

Luego de esto, deberá abrir el `cmd` \(Win + R y escriba "cmd" en ejecutar\) y abrirá la ruta donde se encuentra la carpeta con el comando `cd`, ejemplo:
```console
cd desktop/bot
```

> **Consejo:** Puede escribir `cmd` en el buscador de carpetas para abrir una consola.

Hecho lo anterior, solo debe ejecutar el siguiente comando: `npm i eris`.
De esta manera, se empezará a instalar `eris` con todas sus dependencias en el directorio seleccionado.

Si no hubo ningún mensaje de error al finalizar la instalación, es hora de programar un bot.

## Primer Bot
Una vez explicados los puntos básicos y teniendo todo listo, podemos empezar a usar `eris`, esta sección cubrirá los primeros pasos en la creación de un bot.

En nuestra carpeta crearemos un archivo con extensión `.js` (Ejemplo: `MiBot.js`) y lo abriremos con cualquier editor de código.

Antes de nada, al igual que con cualquier otro módulo de `Node`, se precisa de la función `require()` para empezar a trabajar con él, así que vamos a declarar una variable que contenga `eris`.

```js
const Eris = require("eris");
```

### Discord Client
Esta clase es el punto principal de interacción con la `discord API`. Debemos definirla y a su vez ingresar el token de nuestro bot.
Con `bot.connect()` iniciaremos nuestro bot.

```js
const bot = new Eris("token");

bot.connect()
```

Para comprobar que el arranque fue exitoso utilizaremos el evento `ready` que es emitido cuando nuestro bot se pone en línea.

```js
bot.on("ready", () => {
  console.log("Estoy list@!")
});
```

Para iniciar nuestro bot debemos escribir el siguiente comando en nuestra consola.

```console
node MiBot.js
```

Si se imprime en la consola el mensaje `Estoy list@!` es porque todo salió bien.

### Recibiendo mensajes
Ahora que nuestro bot está en linea, comprobaremos su funcionamiento intentando que reciba y envíe mensajes, para ello precisamos del evento [`messageCreate`](https://abal.moe/Eris/docs/Client#event-messageCreate), este emitirá un [objeto](https://abal.moe/Eris/docs/Message) con todas las propiedades del mensaje (autor, contenido, canal, etc).

Por ejemplo, haremos que nuestro bot responda al `mensaje ping`.

```js
bot.on("messageCreate", msg => {
  if(msg.content.startsWith() === "ping"){
    bot.createMessage(msg.channel.id, "Pong!");
  }
});
```

En este ejemplo, `msg.content` representaría el contenido del mensaje enviado, utilizando `startsWith()` comprobamos que el mensaje empiece por "ping", de esta manera, solo si se cumple esa condición, enviaría un mensaje que contenga "Pong".

## Comandos
> Dejemos en claro que eris posee un `handler de comandos nativo`. Si quieres ahorrar tiempo o simplemente utilizar el `CommandClient` de eris [salta a este paso](/js/eris/commandclient.md).

Para empezar definiremos nuestro prefijo e ignoraremos los mensajes enviados por cualquier bot.

```js
const prefix = "!"

if(message.author.bot) return;
```

Una vez echo esto, haremos que nuestro bot elimine el prefijo del mensaje, dejando solo el comando y tomaremos el resto del mensaje como **argumentos**.

```js
const args = message.content.slice(prefix.length).trim().split(/ +/g);
const command = args.shift().toLowerCase();
```

En la variable `args` tomamos el contenido del mensaje. Utilizando `slice()` eliminamos el prefijo, para luego con `trim()` eliminar posibles espacios en blanco. Con `split(/ +/g)` separamos el mensaje en espacios blanco, dejando solo un array.

Luego con `command` definimos los comandos. Eliminamos el primer elemento de un array con `shift()` (en este caso el array es `args`), y luego indicamos que el comando sea insensitivo al caso con `toLowerCase()`.

```js
if (command === "mipais") {
  bot.createMessage(msg.channel.id, "Tu país es: "+args[0]);
  return;
}
```

En este ejemplo estamos creando un comando que devolverá el país especificado por el usuario. Su uso sería `!mipais Argentina`, por lo que el bot devolverá `Argentina`. Este es un ejemplo básico de como usar argumentos, puedes extenderte tanto como quieras y crear cosas muy interesantes.

Hasta aquí todo bien, pero si el usuario utilizará dos argumentos (ej: `!mipais Argentina Venezuela`) solo retornará `Argentina`. Para hacer que retorne todos los argumentos especificados utilizaremos `join()`.
```js
if (command === "mipais") {
  var content = args.join(" ");
  bot.createMessage(msg.channel.send, "El contenido del mensaje es: "+content);
  return;
}
```

Entonces al utilizar `!mipais Yo vivo en Argentina` el bot enviará `Yo vivo en Argentina`.

Siguiente sección [CommandClient](/js/eris/commandclient.md)
