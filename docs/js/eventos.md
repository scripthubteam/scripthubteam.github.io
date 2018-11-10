
* [¿Qué es un evento?](#¿qué-es-un-evento)
* [Usando eventos](#usando-eventos)
* [Listado de eventos](#listado-de-eventos)


## ¿Qué es un evento?

!> **Este es un reforzamiento del concepto de Eventos de discord.js que viste en [Primeros Pasos](/js/Primeros_Pasos.md)**. Si no lo has leído todavía, por favor hazlo. Si ya entiendes los pasos básicos de discord.js, puedes seguir en esta página.

**discord.js** dispara un evento cada vez que sucede algo. Por ejemplo, un evento se activa cuando se crea, borra o edita un mensaje. Un evento se activa cada vez que se crea, borra o edita un canal.

Cuando desee que su bot reaccione ante un evento, puede agregar un controlador de eventos (también denominado detector de eventos).

>Como nota rápida, en esta guía puede ver "ES6" y preguntarse a qué nos estamos refiriendo. JavaScript también se conoce como ECMAScript, y ES6 es una versión del mismo.
>
>Puede encontrar más información sobre las funciones de flecha, a las que se hace referencia a continuación, en la [Mozilla Developer Network.](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Functions/Arrow_functions)

```js
// Las dos opciones siguientes hacen lo mismo y se pueden intercambiar


// 1) ES6 introduce funciones de flecha más cortas y opcionales
client.on('message', message => {

});

// 2) Funciones normales si no estás usando ES6 o superior
client.on('message', function(message) {
    
});
```

## Cómo utilizar eventos

Nos iremos a la [Documentación oficial de Discord.js](https://discord.js.org/#/docs/main/stable/class/Client) y nos ubicaremos en la sección de `Events`, luego buscaremos el evento _"typingStart"_ en la lista.

![](https://i.imgur.com/8P4IeeY.png)

Ya encontrado _"typingStart"_ en la lista. En la descripción nos dice que el evento
se dispara cada vez que un usuario comienza a escribir en un canal.

![](https://i.imgur.com/twFyf7O.png)

La tabla de parámetros puede ser confusa para algunos. Sin embargo, solo nos dice los parámetros que son
suministrado a nuestro controlador de eventos. El orden de estos parámetros es importante. A continuación
un controlador de eventos de ejemplo:

La columna "Type" también puede confundirte. Esta es la [clase](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Classes)
que el parámetro proporcionado es una _instancia_.
Significa que el parámetro es más que un objeto normal, también contiene propiedades y funciones
que puedes usar. Por ejemplo, los objetos _Channel_ tienen la propiedad `name` (es decir `channel.name`) y también puede eliminar un canal
de Discord usando `channel.delete ()`.

```js
// Las dos opciones siguientes hacen lo mismo y se pueden intercambiar


// 1) ES6 introduce funciones de flecha más cortas y opcionales
client.on('typingStart', (channel, user) => {
  console.log(`${user.username} está escribiendo en ${channel.name}`);
});

// 2) Funciones normales si no estás usando ES6 o superior
client.on('typingStart', function(channel, user) {
  console.log(user.username + ' está escribiendo en ' + channel.name);
});
```
## Listado de ejemplos de eventos

!> Haga click encima de un evento para ser llevado a una explicación del mismo.


| **Evento** | **Descripción** |
| :------ | :------: |
| [ready](https://discord.js.org/#/docs/main/stable/class/Client?scrollTo=e-ready) | Se emite cuando el cliente está listo para comenzar a trabajar.
| [message](https://discord.js.org/#/docs/main/stable/class/Client?scrollTo=e-message) | Se emite cada vez que se crea un mensaje.
| [guildMemberAdd](https://discord.js.org/#/docs/main/stable/class/Client?scrollTo=e-guildCreate) | Emitido cada vez que el cliente se une a un servidor/grupo.
| [guildMemberRemove](https://discord.js.org/#/docs/main/stable/class/Client?scrollTo=e-guildDelete) | Emitido cada vez que se elimina un grupo/salido.
| [guildMemberUpdate](https://discord.js.org/#/docs/main/stable/class/Client?scrollTo=e-guildMemberUpdate) | Se emite cada vez que un miembro del servidor cambia, es decir, un nuevo rol, rol eliminado, apodo.
| [guildCreate](https://discord.js.org/#/docs/main/stable/class/Client?scrollTo=e-guildCreate) | Emitido cada vez que el cliente se une a un grupo.
| [guildDelete](https://discord.js.org/#/docs/main/stable/class/Client?scrollTo=e-guildDelete) | Emitido cada vez que se elimina un grupo/sale. 
| [error](https://discord.js.org/#/docs/main/stable/class/Client?scrollTo=e-error) | Se emite cada vez que el WebSocket del cliente encuentra un error de conexión.
| [warn](https://discord.js.org/#/docs/main/stable/class/Client?scrollTo=e-warn) | Emitido para advertencias generales.
| [debug](https://discord.js.org/#/docs/main/stable/class/Client?scrollTo=e-debug) | Emitido para información general de depuración.

## Evento "ready"

El evento *ready* nos puede servir a muchos desarrolladores a saber cuándo el bot ha podido conectarse correctamente con Discord, pues el proceso para saber utilizar este evento es sumamente fácil si queremos enviar una alerta a nuestra consola de que el bot se ha podido conectar con Discord.

**Este evento es importante.** Este evento te ayudará a saber cuándo tu bot se conecta con Discord.

A continuación, un pequeño ejemplo de un evento *ready* básico:

```js
/** Evento ready **/

client.on("ready", () => {
  client.user.setActivity("¡Estoy conectado!"); //Cambia su actividad (juego) a "¡Estoy conectado!"
  console.log("¡Conectado! Estoy en "+client.guilds.size+" servers, con "+client.users.size+" users en total."); //Envía un mensaje a la consola cuando se conecte con Discord.
});
```

## Evento "message"

El evento *message* es útil cuando queremos obtener todo lo que se escribe en un canal (*channel*), con este evento podemos obtener el autor de un mensaje y lo más importante, **crear comandos**.

Suena lógico desde este punto, un evento para obtener mensajes y que nos sirve para empezar a crear nuestros primeros comandos.

A continuación, un pequeño y sencillo ejemplo de una estructura básica del evento:

```js
/** Evento message **/

client.on('message', async message => {
  const client = message.client;
  const args = message.content.slice(prefix.length).trim().split(/ +/g);
  const command = args.shift().toLowerCase();
  var prefix = "s!" // El prefix de tu bot, por ejemplo "s!"

  if (!message.content.startsWith(prefix) || message.author.bot) return; // Los operadores || que seguro ya conoces, permiten que si una de estas dos condiciones se ejecutan, posteriormente retornen un valor falso. El verdadero proceso de estas condiciones es ignorar un mensaje que no contenga el prefix del bot/los mensajes de los bots.

  try{

  	if(command === "help"){
  		message.channel.send("Hola!") //Amablemente tu bot te responderá con un "Hola!" cuando utilices el comando "s!help". Muy tierno~
  		return;
  	}

  } catch(e) { //Atrapa errores dentro de la operación try{}, que en teoría es donde están ubicados los comandos.
  	console.log(e)
  }

})
```

## Evento "guildMemberAdd"

Con este evento podemos saber qué usuario entró al servidor y obtener el objeto del mismo para usarlo de la manera que nosotros deseemos.

Explicaremos a continuación un ejemplo sencillo para que un bot presente una *bienvenida*:

```js
/** Evento guildMemberAdd **/

client.on("guildMemberAdd", async function(client, member){
  let canal = client.channels.get("[ID DEL CANAL PARA MOSTRAR LA BIENVENIDA]")
  let embed = {
  "title": "¡Bienvenido "+member.user.tag+" a "+member.guild.name+"!",
  "description": "Pásala bien y tómate el tiempo de leer las reglas.",
  "color": 0x36393e,
  "timestamp": new Date(),
  "footer": {
    "text": member.guild.name+" - N#"+member.guild.memberCount
  },
  "image": {
    "url": "https://i.imgur.com/IfPahSm.png"
  }
};
canal.send({embed})
  })
```

## Evento "guildMemberRemove"

Al igual que el evento anterior, lo contrario. Con este evento podremos saber con exactitud qué usuario sale de un servidor.

Explicaremos a continuación un ejemplo sencillo si queremos que el bot ahora *despida* un usuario:

```js
/** Evento "guildMemberRemove" **/

client.on("guildMemberRemove", async function(client, member){
  let canal = client.channels.get("[ID DEL CANAL PARA MOSTRAR LA DESPEDIDA]")
  let embed = {
  "title": "¡Adiós "+member.user.tag+"! Esperamos volver a verte por "+member.guild.name+".",
  "description": "Pásala bien y tómate el tiempo de leer las reglas.",
  "color": 0x36393e,
  "timestamp": new Date(),
  "footer": {
    "text": member.guild.name+" - N#"+member.guild.memberCount
  },
  "image": {
    "url": "https://i.imgur.com/IfPahSm.png"
  }
};
canal.send({embed})
  })
```

## Evento "guildMemberUpdate"

Este evento es útil cuando queremos obtener la nueva información de un usuario actualizado, es decir, podemos retornar el obtejo antes de su actualización y después de esta.

Explicaremos a continuación un ejemplo si queremos que el bot obtenga el apodo o username de un usuario y le otorgemos un rol si usa un determinado carácter:

```js
/** Evento "guildMemberUpdate" **/

client.on("guildMemberUpdate", async function(oldMember, newMember){
/** 
oldMember = Información anterio del usuario
newMember = Información nueva del usuario
**/

// Ejemplo de cambios de Apodo
if(newMember.nickname === "Test"){ //Si el usuario se cambió el apodo a "Test" le otorgará un rol que debes especificar abajo
newMember.roles.add(newMember.guild.roles.find("ID del rol a otorgar"))
newMember.user.send("Se te otorgó un rol por cambiarte el apodo a Test.")
return;
}

//Ejemplo de cambio de @Username
if(newMember.user.username === "Test"){ //Si el usuario se cambió el @username a "Test" le otorgará un rol que debes especificar abajo
newMember.roles.add(newMember.guild.roles.find("ID del rol a otorgar"))
newMember.user.send("Se te otorgó un rol por cambiarte el username a Test.")
return;
}

//Ejemplo por tener un carácter en el nick
if(newMember.nickname.indexOf("!") !== -1){ // Si un miembro tiene el signo "!" en su apodo, se le otorgará un rol que debes especificar abajo
newMember.roles.add(newMember.guild.roles.find("ID del rol a otorgar"))
newMember.user.send("Se te otorgó un rol por contener el carácter "!" en tu apodo.") //Le enviará un mensaje privado al usuario
return;
}
})
```

## Evento "guildCreate"

Este evento es sencillo de usar, precisará una información cuando un bot tiene una nueva presencia en un servidor, es decir, entrar a ese servidor. **Nota:** ¡No confunder con `guildMemberAdd`! Este evento trata el ingreso del bot, no de los usuarios.

```js
client.on('guildCreate', guild =>{
  console.log("[GUILD] ▶ Unido a "+guild.name+" ("+guild+") el ["+new Date()+"]")
});
```

## Evento "guildDelete"

Lo contrario al evento anterior, información del servidor de expulsión del bot, más específicamente cuando este sale o deja de ser parte del mismo.

```js
client.on('guildDelete', guild =>{
  log("[GUILD] ◀ Expulsado de "+guild.name+" ("+guild+") el ["+new Date()+"]")
});
```

## Evento "messageReactionAdd"

Evento disparado al ser reaccionado uno de los mensajes en la caché del bot. El primer parámetro devuelve un objeto MessageReaction que incluye Message si necesitarámos especificar un determinado mensaje.

```js
client.on("MessageReactionAdd", (reaction, user) => {
    if (reaction.emoji.name === "verificado") {
        user.username.addRole('ID'));
    }
});
```

## Evento "raw"

Se dispara este evento ante cualquier acción recibida desde los servidores de Discord. Nótese que no estará "preprocesada" por Discord.js pero ofrecerá la máxima flexibilidad.

```js
client.on('raw', event => {
    console.log('Evento disparado', event);
});
```
## Evento "error"

Este evento es opcional, pero su utilización también es útil para enviar detalles de un error producido dentro del bot.

```js
client.on("error", (e) => console.error(e));
```

## Evento "warn"

Este evento también es opcional, es emitido para advertencias generales del sistema.

```js
  client.on("warn", (e) => console.warn(e));
```

## Evento "debug"

Este evento proporciona datos avanzados para el desarrollador, en caso de que quieras probar algunas funciones y obtener respuestas del proceso.

```js
  client.on("debug", (e) => console.info(e));
```
