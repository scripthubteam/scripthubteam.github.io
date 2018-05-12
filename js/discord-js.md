# Índice

* [**Introducción**](##introducción) - Introducción de la librería `discord.js` y preparación del entorno de trabajo	
  * [**Requisitos**](###requisitos) - Software necesario para empezar a utilizar `discord.js`	
  * [**Instalación**](###instalacion) - Instalación de la librería	


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
