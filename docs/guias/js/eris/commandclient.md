
## CommandClient: Comandos con estilo
Eris posee una clase muy práctica llamada `CommandClient`, la cual es muy útil ya que nos permite moldear nuestros comandos de una forma rápida y eficáz. También nos ahorramos el escribir líneas de código para utilidades como el cooldown, aliases, etc.

Para utilizar esta clase debemos hacer unas modificaciones en la declaración de la clase `Client`.

```js
const bot = new Eris.CommandClient("TÚ_TOKEN_AQUÍ", {}, {
  description: "Un bot escrito para Script Hub",
  owner: "Lau",
  prefix: ["!", ","]
});
```

De esta forma estamos definiendo nuestra clase Client, pero con algunos parámetros que actúan como opciones. Estas tienen diversos usos, como definir un `prefijo` o en este caso dos de los mismos.

> Para ver todas las opciones disponibles revisa ["CommandClient"](https://abal.moe/Eris/docs/CommandClient)

### Definiendo un comando
Ya configuramos nuestro CommandClient con las opciones básicas para nuestro bot, así que ahora toca definir nuestro primer comando.
Para ello utilizaremos el método `registerCommand`, proveniente de la clase `CommandClient`.

```js
bot.registerCommand("ping", "Pong!", {
  description: "Envia un mensaje diciendo Pong!",
  fullDescription: "Este comando puede usarse cuando estás aburrido."
});
```

En este ejemplo estamos registrando un comando llamado `ping`.
En el segundo parámetro estamos especificando que retorne `Pong!` cuando el comando se ejecute.
En el tercer parámetro definimos opciones, tal como en la clase `CommandClient`.

> Para ver todas las opciones disponibles revisa ["registerCommand"](https://abal.moe/Eris/docs/CommandClient#function-registerCommand)

También podríamos enviar un mensaje al azar escribiendo el segundo parámetro como `array`.

```js
bot.registerCommand("ping", ["Pung!", "Pang!", "Pong!"], {
  description: "Envia un mensaje al azar. Puede ser Pung!, Pang! o Pong!",
  fullDescription: "Este comando puede usarse cuando estás aburrido."
});
```

### Utilizando argumentos
Hacer un comando simple es muy fácil, pero cuando se trata de comandos con argumentos... ¡sigue siendo muy fácil!

Supongamos que queremos programar un comando en donde el usuario pueda contarnos un poco de su vida.

```js
var sobremi = bot.registerCommand("sobremi", (msg, args) => {
    if (args.length === 0) return "Cuéntanos sobre tu vida!";
    var miVida = args.join(" ");
    return miVida;
}, {
    aliases: ["contarvida"],
    description: "Envia un mensaje contandonos tu vida!",
    fullDescription: "Este comando puede usarse cuando estás aburrido.",
    cooldown: 10,
    cooldownExclusions: { userIDs: ["595734746059898927"] },
    cooldownMessage: "Este comando está en cd.",
    usage: "Pequeño texto sobre tu vida",
    invalidUsageMessage: "Uso inválido"
});
```

Su uso sería algo como esto: `!sobremi Mi vida es muy divertida!`. El bot enviaría un mensaje con el contenido de la vida del usuario al canal en donde el comando fue utilizado. Si el usuario no nos cuenta nada, le diremos `Cuéntanos sobre tu vida!`.

En este ejemplo me decanté por utilizar `join(" ")`, que es el método encargado de unir todos los elementos del array `args` con espacios en blanco.
Al ser `args` un array podría utilizar `args[0]` para tomar el `string` que se encuentre en la posición 0.

```js
var sobremi = bot.registerCommand("sobremi", (msg, args) => {
    if (!args[0]) return "Cuéntanos sobre tu vida!";
    return args[0]
}, {
    aliases: ["contarvida"],
    description: "Envia un mensaje contandonos tu vida!",
    fullDescription: "Este comando puede usarse cuando estás aburrido.",
    cooldown: 10,
    cooldownExclusions: { userIDs: ["595734746059898927"] },
    cooldownMessage: "Este comando está en cd.",
    usage: "Pequeño texto sobre tu vida",
    invalidUsageMessage: "Uso inválido"
});
```

Supongamos que el usuario utiliza `!sobremi mi vida es genial`. En este caso solo enviaría el `mi`, ya que se encuentra en la posición 0.

Además podemos ver que en los dos ejemplos agregué muuuuchos más parámetros de configuración del comando. A continuación los explicaré.

 * El parámetro `aliases` sirve para designar aliases para tu comando. Por ejemplo, el comando `sobremi` también podría ser utilizado como `contarvida`.
 * `description` mostrará la descripción del comando al utilizar el ["comando help"](####Comando-help).
 * `fullDescription` mostrará una descripción más detallada del comando al utilizar `help <comando>`.
 * Con `cooldown` definiremos el tiempo de espera del comando.
 * `cooldownExclusions` permite definir que usuarios, servidores o canales serán exlucídos del cooldown.
  * `userIDs` es un array con las ID's de usuarios que no se verán afectados por el cooldown.
 * `cooldownMessage` define el mensaje que retornará en caso de que el usuario intente usar un comando en cooldown.
 * `usage` mostrará el uso en el comando help.
 * `invalidUsageMessage` define el mensaje de retorno en caso de que el `usage` esté mal utilizado.

### Comando help
La clase `CommandClient` nos provee de un comando help predefinido. No hace falta escribir ni **una sola línea de código** para comenzar a usar este comando, simplemente tipea tu `prefijo` seguido de `help`.

Muchos de los parámetros que especificaras mientras creas tus comandos serán mostrados en el comando help. Por ejemplo, el parámetro `description` mostrará una descripción breve del comando.

> **Nota:** Si quieres desactivar este comando debes especificar `defaultHelpCommand: false` en los parámetros de la constante `bot`.

Este como es de esperarse está escrito en inglés, pero modificarlo es muy sencilo.

Debemos ingresar a nuestra carpeta `node_modules` y buscar el directorio de `eris`. Una vez dentro ingresaremos a la carpeta `lib` para luego ingresar al directorio `command`.
Una vez hecho esto veremos un archivo llamado `CommandClient` el cual deberemos abrir con nuestro editor de código.
Ahora debemos encontrar `if(this.commandOptions.defaultHelpCommand) {` **(Puede utilizar la herrmienta Buscar/Find de su editor de código)** o dirigirse a la `línea 74` **(Ahí debería encontrarse lo que necesitamos)** y reemplazar el contenido **(Línea 74 - 127)** con el contenido del bloque de código que se anexa debajo.

```js
if(this.commandOptions.defaultHelpCommand) {
    this.registerCommand("help", (msg, args) => {
        let result = "";
        if(args.length > 0) {
            let cur = this.commands[this.commandAliases[args[0]] || args[0]];
            if(!cur) {
                return "Comando no encontrado";
            }
            let {label} = cur;
            for(let i = 1; i < args.length; ++i) {
                cur = cur.subcommands[cur.subcommandAliases[args[i]] || args[i]];
                if(!cur) {
                    return "Comando no encontrado";
                }
                label += ` ${cur.label}`;
            }
            result += `**${msg.prefix}${label}** ${cur.usage}\n${cur.fullDescription}`;
            if(cur.aliases.length > 0) {
                result += `\n\n**Aliases:** ${cur.aliases.join(", ")}`;
            }
            const subcommands = Object.keys(cur.subcommands);
            if(subcommands.length > 0) {
                result += "\n\n**Subcomandos:**";
                for(const subLabel of subcommands) {
                    if(cur.subcommands.hasOwnProperty(subLabel) && cur.subcommands[subLabel].permissionCheck(msg)) {
                        result += `\n  **${subLabel}** - ${cur.subcommands[subLabel].description}`;
                    }
                }
            }
        } else {
            result += `${this.commandOptions.name} - ${this.commandOptions.description}\n`;
            if(this.commandOptions.owner) {
                result += `desarrollado por ${this.commandOptions.owner}\n`;
            }
            result += "\n**Comandos:**\n";
            for(const label in this.commands) {
                if(this.commands.hasOwnProperty(label) && this.commands[label] && this.commands[label].permissionCheck(msg) && !this.commands[label].hidden) {
                    result += `  **${msg.prefix}${label}** - ${this.commands[label].description}\n`;
                }
            }
            result += `\nEscribe ${msg.prefix}help <comando> para más información sobre un comando.`;
        }
        return result;
    }, {
        description: "Este es el comando de ayuda.",
        fullDescription: "Este comando se usa para ver información de diferentes comandos de bot, incluido este."
    });
    if(!this.commandOptions.defaultCommandOptions.invalidUsageMessage) {
        this.commandOptions.defaultCommandOptions.invalidUsageMessage = "Invalid usage. Do `%prefix%help %label%` to view proper usage.";
    }
} else if(!this.commandOptions.defaultCommandOptions.invalidUsageMessage) {
    this.commandOptions.defaultCommandOptions.invalidUsageMessage = "Invalid usage.";
}
}
```
> **Nota:** Lo que no esté traducido es porque puede modificarse mediante parámetros al definir `CommandClient`.

### Sub comandos
Esta clase actúa como un registro de comando normal, solamente cambiará el hecho de que deberá ser utilizado como un `subcomando`. (Ej: *!comando subcomando*)
Tomemos el ejemplo del comando ["sobremi"](###utilizando-argumentos) y especifiquemos que si el `subcomando` es `pais` retorne el país especificado en los argumentos.

```js
sobremi.registerSubcommand("pais", (msg, args) => {
  if (args[0]) return "Cuál es tu país de orígen?";
  return args[0]
}, {
  aliases: ["mipais"],
  description: "Cuentas tu país",
  fullDescription: "Este comando puede usarse cuando estás aburrido.",
  cooldown: 10,
  cooldownExclusions: { userIDs: ["595734746059898927"] },
  cooldownMessage: "Este comando está en cd.",
  usage: "Muestranos tu país",
  invalidUsageMessage: "Uso inválido"
});
```

Entonces con `sobremi.registerSubcommand()` estamos registrando un sub comando al comando `sobremi`. Al utilizar `!sobremi pais Argentina` el bot, como es de esperar enviaría `Argentina` al chat.

> **Atención!** Esta es una versión primitiva de la guía sobre *Eris*. Seguiremos actualizandola con más contenido.


