# Índice

1. [Introducción](#introducción) - ¿Por qué Heroku?
2. [Instalación de Heroku CLI](#heroku-cli) - Interfaz de uso de Heroku.
    * [Características de Heroku CLI](##características) - Herramientas más usadas y curiosidades.
    * [Crear Aplicación con Heroku](##crear-aplicación) - Cómo crear una aplicación con Heroku CLI.
3. [Preparación del Bot](#preparando-el-bot) - Explicación de los entornos necesarios.
    * [Java](##bot-en-java) - Preparación para el Bot en Java.
    * [Python](##bot-en-python) - Preparación para el Bot en Python.
4. [Añadiendo Procfile](#añadir-procfile) - Indicaciones a Heroku sobre cómo se ejecuta nuestro Bot.

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

## Bot en Java

Heroku es capaz de trabajar aplicaciones Java construídas tanto en entornos **Maven** como **Gradle**, pero en este tutorial en concreto solo enseñaremos a configurar un entorno Maven.

### Requisitos

- [JDK](http://www.oracle.com/technetwork/java/javase/downloads/index.html) (Cualquier versión debería valer, aunque el estándar es JDK8)
- [JRE](http://www.oracle.com/technetwork/java/javase/downloads/index.html) (De la misma versión que el JDK)
- [Maven](https://maven.apache.org/download.cgi)

### Preparando el entorno Maven

Lo primero será comprobar que *Maven* está correctamente instalado, y para ello hacemos uso del comando `mvn --version`, obteniendo un resultado parecido al siguiente:

![comprobar-maven](https://i.imgur.com/AxCAtum.png)

Los proyectos Maven se estructuran de una manera particular basada en el siguiente esquema:

```
my-app (Carpeta)
|-- pom.xml (Archivo con información sobre el proyecto)
`-- src (Carpeta)
    |
    `-- main (Carpeta)
    |   |
    |   `-- java (Carpeta contenedora de los archivos .java)
    |       
    `-- test (Carpeta)
        `-- java (Carpeta contenedora para los JUnit Test)
```

Es decir, basándonos en la aplicación antes creada (`scripthubteam-test-app`), el árbol de carpetas quedaría tal que:

```
scripthub-test-app (Carpeta de Aplicación clonada con Git)
`-- src
    |
    `-- main
    |   |
    |   `-- java
    |       
    `-- test
        `-- java
```

![tree-powershell](https://i.imgur.com/ga3nsjY.png)

>Estas carpetas pueden ser creadas con un *Goal* de Maven, pero en este caso han sido creadas manualmente para clarificar el ejemplo.

Una vez creadas las carpetas, bastará con crear el archivo `pom.xml` para que nuestra aplicación sea legible por Maven.  
Este archivo XML debe contener una serie de especificaciones:

**1. Etiqueta de apertuda del proyecto y etiqueta de cerrado, la de apertura con los NameSpaces correspondieste al POM y su versión.**

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd">

</project>
```

>En este caso, como se puede denotar, estamos usando la versión 4.0.0 del POM.

**2. Ahora procederemos a añadir la etiqueta de versionado, y ciertos elementos de identificación de nuestra aplicación.**

>Primero se expondrá el resultado final y luego se explicará etiqueta a etiqueta.

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd">
  <!-- Modelo de versionado -->
  <modelVersion>4.0.0</modelVersion>

  <!-- Identificadores del Bot -->
  <groupId>scripthub.bot</groupId>
  <artifactId>TestBot</artifactId>
  <packaging>jar</packaging>
  <version>0.1</version>
  <name>ScriptHub Test Bot</name>
  <url>http://scripthubteam.github.io/docs/</url>

</project>
```

* `<modelVersion>` - Hace referencia al modelo de POM usado en el proyecto.
* `<groupId>` - Indica el grupo identificador del proyecto.
* `<artifactId>` - Señala el identificador de ese proyecto concreto dentro del grupo.
* `<packaging>` - Indicamos el tipo de archivo que queremos obtener tras la compilación.
* `<version>` - La versión de nuestra aplicación.
* `<name>` - El nombre de nuestra aplicación a nivel MANIFEST (cosas del JAR).
* `<url>` - Indica la URL en la que encontrar más información sobre nuestro proyecto.


>Es recomendado que groupId sea también la/s carpeta/s contenedoras del proyecto, es decir, si nuestro groupId es **scripthub.bot**, nuestro árbol de carpetas sería tal que:
>![arbol-scripthub](https://i.imgur.com/d5z452q.png)
>Aunque no es del todo necesario, es una buena práctica.

**3. Señalamos el compilador que vamos a usar (la versión del JDK) y el tipo de codificación (lo más normal es usar UTF-8).**

>Como en este caso en particular se está usando JDK8, el compilador será 1.8, si fuera JDK10 el compilador sería 10, etc...

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd">
  <!-- Modelo de versionado -->
  <modelVersion>4.0.0</modelVersion>

  <!-- Identificadores del Bot -->
  <groupId>scripthub.bot</groupId>
  <artifactId>TestBot</artifactId>
  <packaging>jar</packaging>
  <version>0.1</version>
  <name>ScriptHub Test Bot</name>
  <url>http://scripthubteam.github.io/docs/</url>

  <!-- Compilador y Codificación -->
  <properties>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    <maven.compiler.source>1.8</maven.compiler.source>
    <maven.compiler.target>1.8</maven.compiler.target>
  </properties>

</project>
```

**4. Añadimos las dependencias, que en este caso solamente es el JDA.**

>Al usar JDA como dependencia, también debemos añadir el repositorio jcenter, ya que de otra manera no encontrará la dependencia.
>Además, en la sección `<version>` de JDA se deberá exponer la versión usada, en este caso se está usando la **3.7.1_388**.

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd">
  <!-- Modelo de versionado -->
  <modelVersion>4.0.0</modelVersion>

  <!-- Identificadores del Bot -->
  <groupId>scripthub.bot</groupId>
  <artifactId>TestBot</artifactId>
  <packaging>jar</packaging>
  <version>0.1</version>
  <name>ScriptHub Test Bot</name>
  <url>http://scripthubteam.github.io/docs/</url>

  <!-- Compilador y Codificación -->
  <properties>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    <maven.compiler.source>1.8</maven.compiler.source>
    <maven.compiler.target>1.8</maven.compiler.target>
  </properties>

  <!-- Dependencias -->
  <dependencies>
    <dependency>
      <groupId>net.dv8tion</groupId>
      <artifactId>JDA</artifactId>
      <version>3.7.1_388</version>
    </dependency>
  </dependencies>

  <!-- Repositorios -->
  <repositories>
    <repository>
      <id>jcenter</id>
      <name>jcenter-bintray</name>
      <url>http://jcenter.bintray.com</url>
    </repository>
  </repositories>

</project>
```

>[JCenter](https://bintray.com/bintray/jcenter) es una página recopilatoria de repositorios, es la web donde se encuentra hosteada la API JDA.

**5. Compilar la aplicación que acabamos de crear. Esto lo haremos mediante el comando `mvn clean install` dado que no hemos señalado ningún tipo de intrucción en el `pom.xml`.**

>Para señalar unas intrucciones de compilación determinadas añadiríamos la etiqueta `<build>` en el `pom.xml` e introduciríamos ahí los pasos correspondientes.

Dentro del directorio de nuestra aplicación ejecutaremos el comando `mvn clean install`, obteniendo una salida parecida a la siguiente:

![output-mvn-compiler](https://i.imgur.com/AijbjDu.png)

El sistema de paqueta **Maven** genera unos archivos de compilación en una caperta apartada de nuestro código fuente, por lo que nuestro árbol de carpetas será modificado y parecido a este:

![arbol-carpetas](https://i.imgur.com/PypVs1Z.png)

**6. Escribir el código necesario para que el Bot funcione.**

Con el proyecto *Maven* ya creado, nuestra aplicación es reconocible por Heroku, pero no tiene código fuente, así que será cuestión de añadir este.  
En este ejemplo usaremos un código bastante simple que solo mostrará por consola.

![encendiendo-bot](https://i.imgur.com/JTsLK6e.png)

Y compilamos de nuevo:  
![compilamos](https://i.imgur.com/lLzBE1d.png)

**7. Añadir las intrucciones de compilación.**

Compilar con Java puede ser un poco complejo, ya que debemos incluir las dependencias de JDA y señalizar la clase Main.  
Debido a esto, haremos uso de algunos plugins que nos ofrece Maven para facilitarnos dicha tarea.

Estos plugins se incluirán en una etiqueta `<build>`, que corresponde a la compilación general, y deberán estar agrupados en una etiqueta `<plugins>`, quedando tal que:

```xml
<build>
  <plugins>
    <plugin>
      <!-- Plugin Info -->
    </plugin>

    <plugin>
      <!-- Otro plugin -->
    </plugin>
  </plugins>
</build>
```

Dejando nuestro `pom.xml` final tal que:

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd">
  <!-- Modelo de versionado -->
  <modelVersion>4.0.0</modelVersion>

  <!-- Identificadores del Bot -->
  <groupId>scripthub.bot</groupId>
  <artifactId>TestBot</artifactId>
  <packaging>jar</packaging>
  <version>0.1</version>
  <name>ScriptHub Test Bot</name>
  <url>http://scripthubteam.github.io/docs/</url>

  <!-- Compilador y Codificación -->
  <properties>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    <maven.compiler.source>1.8</maven.compiler.source>
    <maven.compiler.target>1.8</maven.compiler.target>
  </properties>

  <!-- Dependencias -->
  <dependencies>
    <dependency>
      <groupId>net.dv8tion</groupId>
      <artifactId>JDA</artifactId>
      <version>3.7.1_388</version>
    </dependency>
  </dependencies>

  <!-- Repositorios -->
  <repositories>
    <repository>
      <id>jcenter</id>
      <name>jcenter-bintray</name>
      <url>http://jcenter.bintray.com</url>
    </repository>
  </repositories>

  <!-- Intrucciones de Compilación -->
  <build>
    <plugins>
      <!-- Incluir dependencias JAR -->
      <plugin>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-shade-plugin</artifactId>
        <version>1.6</version>
        <executions>
          <execution>
            <phase>package</phase>
            <goals>
              <goal>shade</goal>
            </goals>
          </execution>
        </executions>
      </plugin>
    
      <!-- Incluir clase Main -->
      <plugin>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-jar-plugin</artifactId>
        <configuration>
          <archive>
            <manifest>
              <addClasspath>true</addClasspath>
              <mainClass>scripthub.bot.Main</mainClass>
            </manifest>
          </archive>
        </configuration>
      </plugin>
    </plugins>
  </build>

</project>
```

* `maven-shade-plugin`: Este plugin recopila todas nuestra dependencias y las incluye dentro de nuestro JAR final, dado que de otra manera la palicación no podría ser ejecutada.
* `maven-jar-plugin`: Este plugin maneja el tipo de MANIFEST que tendrá el JAR final.  
  - `<addClasspath>`: Añade el *Classpath*.
  - `<mainClass>`: Señala la clase *Main*.

Cambiamos un poco el código de la clase Main para que sea propiamente un bot ejecutable.

![bot-code](https://i.imgur.com/C9zcaYP.png)

Y volvemos a compilar:  
![compilar-bot](https://i.imgur.com/dSCHP7R.png)

>Al ser compilado como archivo JAR, este puede ser ejecutado con la sencilla instrucción `java -jar [archivo .jar]`, en este caso sería `java -jar target\TestBot-0.1.jar` debido a que los archivos compilados se guardan en la caperta *target* en los proyectos *Maven*.

Ejecutamos para comprobar que todo funciona:

>En este caso funcionará correctamente excepto por un error debido a que no he puesto un *token*.

![ejecutamos](https://i.imgur.com/okcBNp4.png)

Y listo, ahora solo faltaría subirlo a la plataforma.

---

## Bot en Python

---