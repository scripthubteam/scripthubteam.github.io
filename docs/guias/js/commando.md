
* [¿Qué es Discord.js Commando?](#¿Qué-es-Discord.js-Commando?)
* [Empezando](#Empezando)
* [Primer comando](#Primer-comando)

## ¿Qué es Discord.js Commando?

**Commando es el marco de comando oficial para [discord.js](https://github.com/discordjs/discord.js). Es flexible, 
totalmente orientado a objetos, fácil de usar y hace que sea trivial crear sus propios comandos potentes. Además, hace 
un uso completo de la funcionalidad async / await de ES2017 para un código claro y conciso que es simple de escribir y 
fácil de comprender.**

**Antes de comenzar con esta guía, le recomiendo que eche un vistazo a la guía oficial de Discord.js. Te enseñará los fundamentos de Discord.js, 
que puedes usar para ayudarte a comprender esta guía. Tenga en cuenta que voy a suponer que ha leído esa guía antes de esta. Puedes encontrar esa guía [aquí](http://discordjs.guide/)**

> Nota: si estas aquí sin y aun no sabes que estas haciendo te recomiendo que empieces primero en [Primeros Pasos](/js/Primeros-Pasos.md)

## Empezando

Cuando tienes tu primer bot funcionando con Discord.js, deberías haber instalado Discord.js usando npm, el Administrador de
paquetes de Node.js. Lo mismo se aplica a Commando, que debe instalarse por separado. Puedes hacer esto de una de estas dos formas:

Stable: `npm i -S discord.js-commando`

Master: `npm i -S discordjs/Commando`

Ahora debes crear un archivo con el nombre `index.js`, Si bien no tiene que llamarse index.js, el archivo de índice es su archivo principal para su bot, que maneja todo, desde el registro de nuevos comandos hasta el inicio de sesión en su cliente. Es bastante similar al Discord.js estándar de muchas maneras, pero hay algunos pasos adicionales que se deben hacer para poner en marcha tu bot.

Lo primero es requerir Commando. Contrariamente a lo que pueda pensar, no es necesario que requiera Discord.js para usar Commando. Es decir, a menos que vaya a utilizar `MessageEmbed` porque usare la version master de Discord.js y si tienes la version stable usa `RichEmbed`, pero más sobre eso más adelante.
Por ahora, solo requiere Commando.

```js
const { CommandoClient } = require('discord.js-commando');
const path = require('path');
```

¿Parecer familiar? Eso es casi de la misma manera que requirió Discord.js en su primer bot. El siguiente paso es crear un nuevo Commando Client. También hay algunas opciones que vamos a establecer. Algunos de estos son opcionales, pero recomendados.

```js
const client = new CommandoClient({
    commandPrefix: 'tu prefix aquí',
    owner: 'Tu id aquí',
    disableEveryone: true
});
```

Se ve bastante similar a su cliente Discord.js ¿no? Bueno, aparte de todas las opciones y esas cosas.
En commandPrefix, debes insertar el prefijo que pretendes tener para tu bot. Al momento de escribir, solo puedes tener uno, ¡así que eliges sabiamente! Sin embargo, tenga en cuenta que siempre se permitirá una mención junto con el prefijo que establezca aquí. En otras palabras, este prefijo y menciones son cómo usarás tu bot. No, no hay forma de desactivar las menciones siendo un prefijo!
Después de eso es el campo propietario, que debe contener la identificación de usuario para el propietario del bot. El usuario que configuró aquí tiene control total sobre el bot, puede usar eval y otros comandos de propietario, e ignora la limitación de comandos y los permisos de usuario. Por lo tanto, asegúrese de solo dárselo a usted mismo.
Además, si instaló el maestro anteriormente, esto también puede ser una matriz de ID en lugar de una única.

```js
owner: ['ID', 'ID']
```

La opción final, disableEveryone, simplemente evita que el bot mencione @everyone. Esto es simplemente para evitar que las cosas se vuelvan molestas. Después de todo, ¿quieres que alguien mencione a todos con el bot? No lo creo
A continuación, vamos a registrar los grupos de comandos, los tipos args y los comandos predeterminados, y registrar los comandos en una carpeta llamada `commands`. Puede asignarle el nombre que desee a la carpeta, pero yo personalmente recomiendo `commands`, ya que ... Bueno, tiene más sentido.

```js
client.registry
    .registerDefaultTypes()
    .registerGroups([
        ['group1', 'Our First Command Group']
    ])
    .registerDefaultGroups()
    .registerDefaultCommands()
    .registerCommandsIn(path.join(__dirname, 'commands'));
```

Si desea deshabilitar un comando predeterminado, por ejemplo, el comando de ayuda, puede pasarlo a 
`registerDefaultCommands`.

```js
.registerDefaultCommands({
    help: false
})
```

A continuación, vamos a crear el evento `ready`, que es casi el mismo que el de tu primer Discord.js Bot.

```js
client.on('ready', () => {
    console.log('Logged in!');
    client.user.setActivity('game');
});
```

Esto enviará registrado. a su consola cuando el bot esté listo, y configure el Juego del Bot en 'game'. Ponlo a lo que desees.

Por último, pero no menos importante, vamos a iniciar sesión en el bot.

```js
client.login('Tu token');
```

También puede usar variables de entorno o un config.json para su token en lugar de pasarlo directamente.
¡Y ahí lo tienes! ¡Has configurado tu archivo index.js! Ahora debería crear los comandos y las carpetas group1, al final su estructura de archivos debería verse así, junto con los archivos .gitignore o config.json que pueda tener:

```fix
commands
- group1
index.js
package.json
```

Y su archivo `index.js` debería verse más o menos así:

```js
const { CommandoClient } = require('discord.js-commando');
const path = require('path');

const client = new CommandoClient({
    commandPrefix: 'tu prefix aquí',
    unknownCommandResponse: false,
    owner: 'Tu id aquí',
    disableEveryone: true
});

client.registry
    .registerDefaultTypes()
    .registerGroups([
        ['group1', 'Nuestro primer grupo de comando']
    ])
    .registerDefaultGroups()
    .registerDefaultCommands()
    .registerCommandsIn(path.join(__dirname, 'commands'));

client.on('ready', () => {
    console.log('Logged in!');
    client.user.setActivity('game');
});

client.login('tu token secreto aquí');
```

## Primer comando

Ahora que hemos configurado un grupo de comandos y registrado nuestra carpeta de comandos, ¡estamos listos para crear nuestro primer archivo de comando! Primero, vas a necesitar, por supuesto, crear un nuevo archivo para el comando. Dirígete a la carpeta de `commands` y luego a la carpeta group1 y crea un nuevo archivo llamado reply.js. Este va a ser un comando simple que solo responde con un mensaje cuando se usa. Vamos a entrar en argumentos y todo eso más tarde.
Una vez que tenga su archivo, ¡comencemos!

### Creando tu clase de comando

Antes de hacer nada, al comienzo de su archivo, necesitará requerir comando nuevamente

```js
const { Command } = require('discord.js-commando');
```

Esto permitirá que nuestro comando ... Bien, exista.
Ahora, los comandos son clases exportadas con `module.exports`. Así que creemos la clase y establezcamos `module.exports` en ella.

```js
module.exports = class ReplyCommand extends Command {
    constructor(client) {
        super(client, {
            name: 'reply',
            group: 'group1',
            memberName: 'reply',
            description: 'Replies with a Message.',
            examples: ['reply']
        });
    }
```

No dejes que esto te asuste, en realidad es muy simple.
`name` es el nombre del comando.
`group` es el grupo de comando del que forma parte.
memberName es el nombre del comando dentro del grupo.
`description` es el texto de ayuda que se muestra cuando se usa el comando de ayuda.
`examples` es una serie de ejemplos sobre cómo usar el comando.

#### Creando tu método de ejecución

Lo siguiente que vamos a necesitar es un método de ejecución. Esto debería ir justo debajo del constructor para el comando. Dentro, devolvamos un mensaje:

```js
   run(msg) {
        return msg.say('Hi, I\'m awake!');
    }
};
```
Eso también cerró los corchetes de nuestra clase de comando, ¡así que!
De todos modos, como puede ver, el método de ejecución es simplemente el código que desea que ejecute el bot cuando se usa el comando. Esto puede ser cualquier cosa que puedas hacer en discord.js normales, ya que el comando es simplemente una extensión.
También puede haber notado que utilicé `message.say` en lugar de `message.channel.send`. Esta es la magia del comando. En lugar de enviar, use decir. En lugar de `sendEmbed`, usa incrustar. En lugar de `sendCode`, usa el código, entiendes la idea. La única excepción real en la que puedo pensar es en los archivos, que todavía son message.channel.sendFile.
Entonces, cuando todo esté hecho, su archivo debería verse así:

```js
const { Command } = require('discord.js-commando');

module.exports = class ReplyCommand extends Command {
    constructor(client) {
        super(client, {
            name: 'reply',
            group: 'group1',
            memberName: 'reply',
            description: 'Replies with a Message.',
            examples: ['reply']
        });
    }

    run(msg) {
        return msg.say('Hi, I\'m awake!');
    }
};
``` 

¡Ahora, enciende el bot como siempre y usa tu comando! Debería ser automáticamente <prefix> reply para usarlo.

**Esto es todo por ahora si quieres saber más sobre comando usa la documentacion de discord.js que mencione al principi Owo**
