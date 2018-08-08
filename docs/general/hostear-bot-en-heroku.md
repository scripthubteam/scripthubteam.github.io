# Índice

1. [Introducción](#introducción) - ¿Por qué Heroku?
2. [Instalación de Heroku CLI](#heroku-cli) - Interfaz de uso de Heroku.
    * [Características de Heroku CLI](##características) - Herramientas más usadas y curiosidades.
    * [Crear Aplicación con Heroku](##crear-aplicación) - Cómo crear una aplicación con Heroku CLI.
3. [Preparación del Bot](#preparando-el-bot) - Explicación de los entornos necesarios.
    * [Java](###bot-en-java) - Preparación para el Bot en Java.
    * [Python](###bot-en-python) - Preparación para el Bot en Python.

# Introducción

![imagen-heroku](https://www.vectorlogo.zone/logos/heroku/heroku-card.png)

*¿Por qué Heroku?*  
Heroku es un Hoster usado por gran cantidad de desarrolladores debido a su indiscutible calidad y enorme cantidad de características gratuitas, entre ellas, la capacidad de hostear aplicaciones y asignarles bases de datos.

Heroku ofrece soporte para aplicaciones basadas en los lenguajes **JavaScript (con Node.js), Ruby, Python, Java, PHP, Go, Scala y Clojure**, por lo que abarca gran cantidad de los lenguajes tratados por el equipo **Script Hub**.

# Heroku CLI

Para iniciarnos en Heroku es necesario tener una cuenta en la plataforma y bajarnos su [Interfaz de Consola](https://devcenter.heroku.com/articles/heroku-cli) dado que (a nivel personal) es más cómodo trabajar con esta que con la enrevesada web de Heroku.

La instalación estará llevada por un *Instalador*, por lo que debería ser bastante sencilla (en principio bastará con dejar todo por Default, es decir, todas las casillas marcadas).

Comprobamos que la instalación se ha realizado correctamente abriendo la terminal y escribiendo `heroku -v`, de la que obtendremos algo parecido a esto:

![heroku-terminal](https://i.imgur.com/mwxuSiH.png)

> En caso de que no salga nada o de error al escribir ese comando, seguramente se deba a que este no esté en el *PATH*, por lo que es recomendable buscar información sobre cómo incluirlo en este.

## Características

Si escribimos `heroku` en la terminal, nos saldrá una lista de comandos bastante completa. Los más relevantes son:

- `heroku apps`: *Manejo de nuestras aplicaciones hosteadas en Heroku; desde crearlas y destruirlas hasta compartirlas con un equipo de trabajo.*
- `heroku addons`: *Manejo de los addons relativos a cada aplicación. Heroku tiene gran cantidad de Addons, entre los que se encuentra PostgreSQL, el sistema de Bases de Datos recomendado (ya que los otros que ofrece son de pago).*
- `heroku git`: *Sistema de control de versiones basado en git. El sistema de Heroku ofrece un fácil control del desarrollo de nuestras Apps gracias al uso de la **bandera** `-a`, que permite administrar aplicaciones por su nombre en lugar de su URL.*
- `heroku logs`: *Administra los logs de nuestras aplicaciones.*
- `heroku pg`: *Manejo de las Bases de Datos basadas en PostgreSQL, control del sistema usado, espacio, velocidad...*
- `heroku psql`: *Inicia una terminal de PostgreSQL en la que trabajar.*

>Para obtener más información con cualquiera de los otros comandos bastará con usar la bandera `--help` con cualquiera de estos: `heroku <comando> --help`.

---

## Crear Aplicación

Crear una aplicación con Heroku CLI es tan fácil como proceder a usar el comando `heroku apps:create [nombre] --region=us/eu`, donde `--region` simboliza la región en la que hostear dicha App (solo validan los valores *eu* y *us*).  
Por ejemplo, si quisiéramos crear un App llamada **scripthub-test-app** cuyo host esté en Europa usaríamos el siguiente comando: `heroku apps:create scripthub-test-app --region=eu`, obteniendo un resultado parecido al siguiente:

![crear-app-terminal](https://i.imgur.com/sMV0Qr4.png)

Para comprobar que dicha aplicación ha sido creada bastará escribir `heroku apps`, obteniendo como resultado una lista de nuestras aplicaciones.

![lista-apps-heroku](https://i.imgur.com/ciO0q60.png)

>Como comprobador extra podemos acceder al [Dashboard de Heroku](https://dashboard.heroku.com/apps) y ver nuestras aplicacioens allí.

![lista-apps-dashboard](https://i.imgur.com/4PFgGG4.png)

Para obtener nuestra aplicación de manera local usaremos *Git*, en concreto Heroku con un administrador Git que facilita mucho este trabajo.

>Debido al uso de la bandera (*flag*) `-a` podemos usar cualquier comando de Heroku enfocado a cualquier App simplemente sabiendo el nombre de esta. 

Para clonar el repositorio bastará con usar el comando `heroku git:clone -a [nombre-app]` en el directorio en que queramos clonar dicha App.

Por ejemplo, estando en el directorio actual:

![directorio](https://i.imgur.com/TftYN73.png)

Queremos clonar nuestra app **scripthub-test-app**; usaríamos `heroku git:clone -a scripthub-test-app`:

![heroku-git-clone](https://i.imgur.com/tUlebMl.png)

>El aviso se debe a que el directorio está vacío, es decir, simplemente es una carpeta. Lo cual es lógico si pensamos que hemos creado la App sin ningún tipo de Addon o Plugin, ni hemos señalado el lenguaje que va a usar.

Esa carpeta representa nuestro Bot, por lo que trabajaremos dentro de ella.

# Preparando el Bot

Debido a que cada de lenguaje de programación tiene una manera de preparar el entorno diferente, es necesario separar la preparación de nuestra App según el lenguaje que vayamos a usar.

- [Java](###bot-en-java)
- [Python](###bot-en-python)

### Bot en Java

Heroku es capaz de trabajar aplicaciones Java construídas tanto en entornos **Maven** como **Gradle**, pero en este tutorial en concreto solo enseñaremos a configurar un entorno Maven.

#### Requisitos

- [JDK](http://www.oracle.com/technetwork/java/javase/downloads/index.html) (Cualquier versión debería valer, aunque el estándar es JDK8)
- [JRE](http://www.oracle.com/technetwork/java/javase/downloads/index.html) (De la misma versión que el JDK)
- [Maven](https://maven.apache.org/download.cgi)

#### Preparando el entorno Maven

Lo primero será comprobar que *Maven* está correctamente instalado, y para ello hacemos uso del comando `mvn --version`, obteniendo un resultado parecido al siguiente:

![comprobar-maven](https://i.imgur.com/AxCAtum.png)

---

### Bot en Python

---