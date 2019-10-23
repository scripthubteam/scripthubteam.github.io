
**Git** es un software de control de versiones diseñado por Linus Torvalds, pensando en la eficiencia y la confiabilidad del mantenimiento de versiones de aplicaciones cuando éstas tienen un gran número de archivos de código fuente. Su propósito es llevar registro de los cambios en archivos de computadora y coordinar el trabajo que varias personas realizan sobre archivos compartidos. ([Wikipedia](https://es.wikipedia.org/wiki/Git)).

En resumen, Git es una herramienta que nos permite a nosotros distribuir los cambios de nuestro código a un modelo de *versionamiento* controlado. Esto es muy importante para cuando tenemos nuestros proyectos en un repositorio de nuestro servicio favorito (GitHub, GitLab, BitBucket y etcétera) y queremos tener un registro de cambios a mano por si olvidamos un detalle. Pero lo más importante de esta herramienta es que **nos permite trabajar perfectamente en equipo** y también en colaboración.

### Los tres procesos

Antes de empezar a usar el CLI (*Command Line Interface)* de Git, entendamos los tres procesos que realiza el software:

![](https://git-scm.com/figures/18333fig0106-tn.png)

Git lleva un control moderado a la hora de publicar a la raiz principal nuestro código, pasando de antemano por dos procesos llamados **Working Directory** (*La raíz en donde trabajamos*) y **Staging Area** (*Área de proceso*), hasta finalmente enviarlo al repositorio.

### Descargar Git

* <i class="fas fa-download" title="Descarga Git."></i> [Descargar Git desde su sitio oficial](https://git-scm.com/downloads)

## Creando un repositorio nuevo

Dírigete al directorio deseado y ejecuta `git init` para crear un repositorio de git.

## Añadiendo & Commit

Registra los cambios de tus archivos usando `git add <NombreDeArchivo>` o `git add .` para añadir todos los cambios existentes.

Para agregar un mensaje a tus cambios y empezar añadirlos al *Staging Area*, usa `git commit -m "Mensaje aquí"`.

## Enviando los cambios

Para ir al último paso del versionamiento, utilizaremos `git push -u <cliente> <server>`.
Para añadirlo directamente a la rama `master` (Rama principal predeterminada de nuestro repositorio), usaremos directamente `git push -u origin master`.

**¿No has conectado tu repositorio con el servicio de alojamiento?**

Usa `git remote add origin <server>`, por ejemplo:

**Copiamos el enlace .git de nuestro repositorio**

![](https://i.imgur.com/uIf6dAu.png)

**Lo utilizamos de esta manera en nuestra consola**

![](https://i.imgur.com/awerniJ.gif)

## Las ramas

Para crear una nueva rama en git, utiliza `git checkout -b <nombre>` y posicionate en ella. Para volver a la rama anterior (master) utiliza `git checkout master`, y para borrar una rama en caso de ser necesario utilizaras `git branch -d <nombre>`.

Una nueva rama no estará disponible generalmente hasta que la publiques usando `git push -u origin master`

## Actualizar y fusionar

Para actualizar tu repositorio local con el más reciente utiliza `git pull` y obtendras los cambios hechos externamente de tu directorio local.

Para fusionar una rama con la que está activa utiliza `git merge <rama>`. Antes de fusionar las ramas, puedes usar el **diff** para ver las diferencias de ambas `git diff <rama_fuente> <rama_seleccionada>`.

## Datos misceláneos

* Agrégale color a tu consola con git: `git config color.ui true`
* Mostrar una línea por cada commit: `git config format.pretty oneline`
* Agregar archivos de forma interactiva: `git add -i`

## Referencias

* [git - la guía sencilla](http://rogerdudler.github.io/git-guide/index.es.html)
* [Git - Wikipedia](https://es.wikipedia.org/wiki/Git)
* [Documentación de Git - Fundamentos Generales](https://git-scm.com/doc)
* [Git Community Book](http://book.git-scm.com/)
* [GitHub Help](http://help.github.com/)