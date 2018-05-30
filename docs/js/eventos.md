# Índice

* [¿Qué es un evento?](#¿qué-es-un-evento)
* [Usando eventos](#usando-eventos)
* [Listado de eventos](#listado-de-eventos)


## ¿Qué es un evento?

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

## Usando eventos

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
## Listado de eventos

!> Esta lista se encuentra incompleta temporalmente. Visite la [Documentación oficial de Discord.js](https://discord.js.org/#/docs/main/stable/class/Client?scrollTo=e-channelCreate) para ver una lista completa y actualizada.


A continuación el listado de eventos usables:

| **Evento** | **Descripción** |
| :------ | :------: |
| [ready](https://discord.js.org/#/docs/main/stable/class/Client?scrollTo=e-ready) | Se emite cuando el cliente está listo para comenzar a trabajar.
| [message](https://discord.js.org/#/docs/main/stable/class/Client?scrollTo=e-message) | Se emite cada vez que se crea un mensaje.
| [error](https://discord.js.org/#/docs/main/stable/class/Client?scrollTo=e-error) | Se emite cada vez que el WebSocket del cliente encuentra un error de conexión.
| [typingStart](https://discord.js.org/#/docs/main/stable/class/Client?scrollTo=e-typingStart) | Se emite cada vez que un usuario comienza a escribir en un canal.
| [guildCreate](https://discord.js.org/#/docs/main/stable/class/Client?scrollTo=e-guildCreate) | Emitido cada vez que el cliente se une a un servidor/grupo.
| [guildDelete](https://discord.js.org/#/docs/main/stable/class/Client?scrollTo=e-guildDelete) | Emitido cada vez que se elimina un grupo/salido.