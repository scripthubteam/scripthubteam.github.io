# Índice

1. [Introducción](#introducción) - Breve explicación sobre JDA.
2. [Requisitos](#requisitos) - Programas necesarios para el correcto uso de JDA.
3. [Instalación](#instalación) - Cómo instalar JDA en nuestro sistema.
    - [Maven](###versión-maven) - Instalación con sistema administrador de dependencias Maven.
        * [Eclipse](###eclipse-maven) - Maven para el IDE Eclipse.
        * [NetBeans](###netbeans-maven) - Maven del sistema para el IDE NetBeans.
    - [Gradle](###versión-gradle) - Instalación con el sistema administrador de dependencias Gradle.
        * [Eclipse](###eclipse-jar) - Gradle para el IDE Eclipse.
        * [IntelliJ](###intellij-idea-gradle) - Gradle para el IDE IntelliJ IDEA.
    - [JAR](###versión-jar) - Instalación con el sistema de librerías JAR.
        * [Eclipse](###eclipse-jar) - JAR para el IDE Eclipse.
        * [NetBeans](###netbeans-jar) - JAR para el IDE NetBeans.
        * [IntelliJ](###intellij-idea-jar) - JAR para el IDE IntelliJ IDEA.
4. [Primer inicio](#primer-inicio) - Se hace la explicación de un código simple para ejecutar el bot y esté en línea.
5. [Conclusión](#conclusión) - Se explicará todo lo que se hizo en esta guía de una manera resumida y bien redactada.

## Introducción

[JDA](https://github.com/DV8FromTheWorld/JDA "JDA GitHub") *(Java Discord API)* es un API Wrapper para [Discord API](https://discordapp.com/developers/docs/intro "Documentación de Discord API") que facilita enormemente su uso, es decir, una herramienta para hacer más accesible la API de Discord.

**JDA** fue desarrollado por [Austin Keener](https://github.com/DV8FromTheWorld/ "DV8FromTheWorld GitHub") sirviendo como librería para la creación, manejo y modificación de bots en Java.

## Requisitos

Para hacer uso de la librería **JDA** necesitaremos tener [JDK 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html "JDK 8") o superior instalado en nuestro Sistema.

Podemos comprobar si tenemos **JDK 8** instalando escribiendo `java -version` en la consola de nuestro sistema operativo.

Aunque se puede compilar y ejecutar por consola, en esta explicación usaremos un [IDE](https://es.wikipedia.org/wiki/Entorno_de_desarrollo_integrado "Entorno de desarrollo integrado") (Entorno de desarrollo integrado) para facilitarnos el proceso de usar librerías externas, y se explicará tanto el uso directo de dependencias (archivo JAR), como el uso de proyectos Maven y Gradle.

Se hará un explicación para los IDEs: *Eclipse*, *NetBeans* e *IntelliJ IDEA*. Cualquiera de estos sirve, debes elejir e instalar uno de tu preferencia:
- [Eclipse](https://www.eclipse.org/ide/ "Eclipse")
- [NetBeans IDE](https://netbeans.org/ "NetBeans")
- [IntelliJ IDEA](https://www.jetbrains.com/idea/ "IntelliJ IDEA")

- [JDK 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html "JDK 8")

---

## Instalación

[![JDK](https://img.shields.io/badge/JDK-8+-red.svg)](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html "JDK 8")
[![Maven](https://img.shields.io/badge/Maven-Lastest-yellow.svg)](https://maven.apache.org/ "Maven Download")
[![JDA Version](https://img.shields.io/badge/JDA-Lastest-lightgrey.svg)](https://github.com/DV8FromTheWorld/JDA "JDA GitHub")
[![JDA Wiki](https://img.shields.io/badge/JDA-Wiki-blue.svg)](https://github.com/DV8FromTheWorld/JDA/wiki "JDA Wiki")

- [Instalación con Maven](###versión-maven)
- [Instalación con Gradle](###versión-jar)
- [Instalación con JAR](###versión-jar)

---

### Versión Maven

Es un sistema para construir los JARs de las extensiones (o librerías) de Java para añadirlas a tu proyecto con solo tener el código fuente de estas. Este proceso se hace automáticamente con simplemente la modificación del archivo `pom.xml` creado por Maven.

Partiendo de que un documento `pom.xml` base sería así:
```xml
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
   <modelVersion>Ejemplo</modelVersion>

   <groupId>ejem.plo</groupId>
   <artifactId>Ejemplísimo</artifactId>
   <version>1.0-Ejemplo</version>

   <properties>
      <!-- Más etiquetas con propiedades y/o configuraciones de Maven -->
   </properties>
</project>
```

#### Instalación según el IDE
- [Eclipse](###eclipse-maven)
- [NetBeans](###netbeans-maven)
- [IntelliJ](###intellij-idea-maven)

### Eclipse Maven

- **Se elije el espacio de trabajo (o *workspace*)**.
Abrimos una carpeta cualquiera, no importa si es o no donde estará nuestros archivos del bot, seguido de eso nos dirijimos a `Archivo > Nuevo > Otro > Maven > Proyecto de Maven` (o en inglés, *File > New > Other > Maven > Maven Project*) y ahí sí elejimos la carpeta en donde queremos almacenar todos los datos. Eso empezará a crear un proyecto con el sistema Maven.

- **Configurar algunas cosas de Maven**.
Vamos a empezar a configurarlo, abrimos el archivo `pom.xml` creado automáticamente por nuestra configuración de proyecto, Maven. En ese archivo, como se muestra en el archivo de base, debemos colocar estas etiquetas como configuración o propiedades de Maven:
```xml
<properties>
  <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
  <maven.compiler.source>1.8</maven.compiler.source>
  <maven.compiler.target>1.8</maven.compiler.target>
</properties>
```
Eso hará que el proyecto soporte carácteres UTF-8 y se usa la versión 1.8 en el compilador de Maven.

- **Instalar JDA**.
Ahora agregamos el repositorio de JDA y para eso, colocamos lo siguiente debajo de la etiqueta `</properties>`:
```xml
<repositories>
  <repository>
    <id>jcenter</id>
    <name>jcenter-bintray</name>
    <url>http://jcenter.bintray.com</url>
  </repository>
</repositories>
```
Y añadimos la dependencia de JDA en su versión más reciente, justo debajo de lo anteriormente escrito colocamos:
```xml
<dependencies>
  <dependency>
    <groupId>net.dv8tion</groupId>
    <artifactId>JDA</artifactId>
    <version>4.0.0_52</version>
    <type>jar</type>
    <scope>compile</scope>
  </dependency>
</dependencies>
```

- **¡Listo!** solamente con estos 3 simples pasos ya deberíamos tener instalado JDA por medio de Maven en Eclipse. **Puedes seguir con la guía dándole [clic aquí](#primer-inicio)**.

### NetBeans Maven

- **Crear el proyecto de Maven**.
Debemos crear el proyecto con el sistema Maven, para eso filtramos entre categorías hasta encontrar el que dice `Maven`, ese tiene diferentes tipos de proyectos y seleccionamos `Aplicación de Java` (o *Java Application*) o simplemente `Java`. Eso creará el proyecto con el sistema Maven.

- **Instalar JDA**.
Para instalar el repositorio de JDA en NetBeans, es igual que en Eclipse... Solamente abrimos el archivo `pom.xml` y dentro creamos y/o colocamos las siguientes etiquetas en el espacio libre arriba de `</project>`:
```xml
<repositories>
    <repository>
        <id>central</id>
        <name>bintray</name>
        <url>http://jcenter.bintray.com</url>
    </repository>
</repositories>
```
Seguido de eso, definimos la dependencia por medio de las etiquetas:
```xml
<dependencies>
    <dependency>
        <groupId>net.dv8tion</groupId>
        <artifactId>JDA</artifactId>
        <version>4.0.0_52</version>
    </dependency>
</dependencies>
```

- Y **¡listo!**, ya tendremos nuestro proyecto de Maven con una dependencia instalada, en este caso la de JDA en NetBeans. **Puedes seguir con la guía dándole [clic aquí](#primer-inicio)**.

---

### Versión Gradle

Gradle es muy parecido a Maven, lo único que cambia es la forma en que se escriben las cosas para instalarse.

#### Instalación según el IDE
- [Eclipse](###eclipse-gradle)
- [IntelliJ](###intellij-idea-gradle)

### Eclipse Gradle
> Si tienes instalado *Eclipse para desarrolladores de Java* (*Eclipse for Java Developers*) en tu sistema, salta este paso y ve al paso #2.

- **Abrir Eclipse e ir a la Marketplace o tienda**.
Abrimos una carpeta cualquiera, no importa si es o no donde estará nuestros archivos del bot, seguido de eso nos dirijimos a `Ayuda > Tienda` (o en inglés, *Help > Marketplace*) y buscamos por `Gradle`, e instalaremos el que dice `Buildship Gradle Integration`. Después de haberlo instalado, reiniciamos Eclipse.

- **Creando el proyecto con el sistema Gradle**.
Damos clic derecho sobre el explorador del proyecto, seleccionamos `Nuevo > Otro...` (o en inglés, *New > Other...*) y en la carpeta de `Gradle` seleccionamos `Gradle Project`, el que tiene el símbolo de una letra `G`. Escribimos un nombre para el proyecto y le damos en `Finalizar` (o en inglés, *Finish*).
Si se hizo correctamente todo, el proyecto dentrá una estructura similar a esta:

```
- src/main/java
- src/test/java
- gradle
+ src
    - build.gradle
    - gradlew
    - gradlew.bat
    - settings.gradle
```

Al comprobar si eso es verdadero, eliminamos también las clases `src/main/java` y `src/test/java`.

- **Configurar Gradle e instalar JDA**.
Abriremos el archivo llamado `build.gradle`, eliminamos todo lo que tiene y lo reemplazamos por:

```gradle
plugins {
  id 'java'
  id 'application'
  id 'com.github.johnrengelman.shadow' version '4.0.4'
}

mainClassName = 'clase.principal.o.Main'

version = '1.0'

sourceCompatibility = 1.8

repositories {
  jcenter()
}

dependencies {
  compile 'net.dv8tion:JDA:4.0.0_52'
}

compileJava.options.encoding = 'UTF-8'
```

Debemos reemplazar `clase.principal.o.Main` por el archivo Main o inicial de nuestro bot.
Esto instala JDA en su versión 4.0.0_52 y activa el formato *UTF-8*.

- **¿Listo?** Sííí, eso es todo lo que debemos realizar para instalar JDA en Gradle con la IDE Eclipse. **Puedes seguir con la guía dándole [clic aquí](#primer-inicio)**.

###  IntelliJ Gradle
- **Creamos el proyecto de Gradle**.
Como dice el título, creamos el proyecto de Gradle, para esto, abrimos IntelliJ y clicamos en `Crear nuevo proyecto` (*Create New Project*, en inglés) o nos dirijimos a `Archivo > Nuevo > Proyecto...` (*File > New > Project*, en inglés). En la pestaña o listas de la izquierda elejimos la pestaña que dice `Gradle`, colocamos una SDK de Java en su versión 1.8 y seleccionamos *Java* en la lista de abajo.
Damos clic en siguiente y llenamos el `groupId` y el `artifactId` con cualquier cosa, puede ser `groupId: com.deivid` y `artifactId: BotDePrueba`, la versión no importa demasiado pero la podemos editar.
Activamos la opción `Usar importaciones automáticas` (o *Use auto-import*, en inglés) y damos en siguiente y finalizar.

- **Configurar e instalar Gradle**.
Vamos a darle a conocer a IntelliJ como es nuestro proyecto, por donde debe empezar a ejecutar y entre otras cosas, por medio del archivo `build.gradle`, el cual debemos reemplazar por esto:

```gradle
plugins {
    id'application'
    id'com.github.johnrengelman.shadow' version '4.0.4'
}

mainClassName = 'clase.principal.o.Main'

version '1.0'
def jdaVersion = '4.0.0_52'

sourceCompatibility = targetCompatibility = 1.8

repositories {
    jcenter()
}

dependencies {
    compile "net.dv8tion:JDA:$jdaVersion"
}

compileJava.options.encoding = 'UTF-8'
```

Y debemos reemplazar `clase.principal.o.Main` por el archivo Main o inicial de nuestro bot.
Esto instala JDA en su versión 4.0.0_52 y activa el formato *UTF-8*.

- **¡¿Pero así de fácil?!** Exactamente así de fácil y con únicamente dos pasos se consigue tener Gradle e instalar JDA con este en IntelliJ. **Puedes seguir con la guía dándole [clic aquí](#primer-inicio)**.

---

### Versión JAR

Usar las dependencias del tipo JAR es, básicamente, instalar manualmente una librería descargada.

En este caso se necesitará un JAR que nos provee [JDA](https://github.com/DV8FromTheWorld/JDA "JDA GitHub"). En la sección de [`Releases`](https://github.com/DV8FromTheWorld/JDA/releases "JDA GitHub Releases"), en el primer cuadrado que no sale nos muestra la versión en JAR más reciente, solo debemos bajar un poco a los enlaces de descarga y descargar la que dice `JDA-X.X.X_XXX-withDependencies.jar` (la más reciente al momento de escribir este texto, es la *[JDA-4.0.0_39-withDependencies.jar](https://github.com/DV8FromTheWorld/JDA/releases/download/v4.0.0/JDA-4.0.0_39-withDependencies.jar "Descarga de JDA-withDependencies.jar")*).
Si queremos ayudarnos del autocompletar del IDE y sus anotaciones, debemos descargar los *Sources* y *Javadocs*, encontrados en la misma pestaña de los [`Releases`](https://github.com/DV8FromTheWorld/JDA/releases "JDA GitHub Releases") del GitHub de JDA (en su versión más reciente hasta el momento de escribir esta guía, son: *[JDA-4.0.0_39-sources.jar](https://github.com/DV8FromTheWorld/JDA/releases/download/v4.0.0/JDA-4.0.0_39-sources.jar "Descarga de JDA-sources.jar")* y *[JDA-4.0.0_39-javadoc.jar](https://github.com/DV8FromTheWorld/JDA/releases/download/v4.0.0/JDA-4.0.0_39-javadoc.jar "Descarga de JDA-javadocs.jar")*)

#### Instalación según el IDE
- [Eclipse](###eclipse-jar)
- [NetBeans](###netbeans-jar)
- [IntelliJ](###intellij-idea-jar)

### Eclipse JAR
- **Creamos el proyecto**.
Con la última versión del JAR de JDA descargada, creamos un proyecto de Java común con Eclipse y rellenamos el nombre del proyecto probablemente con el nombre del bot con un texto que diga *"Bot"*, por ejemplo: *"ScriptHubBot"* (no es necesario). En el panel de JRE colocamos la primera opción o la que diga `Usar la ejecución del entorno JRE` (*Use an execution environment JRE*, del inglés), la seleccionamos y establecemos como Java 8 (`JavaSE-1.8`) si está disponible.

- **Añadiendo el JAR de JDA**.
Damos clic derecho en el proyecto y vamos a `Propiedades` (*Properties*, en el inglés), seleccionamos la pestaña `Java Build Path` que está ubicada en la sección lateral izquierda y ahí nos sale en el cuadro de la derecha, otras cuatro pestañas, nos dirigimos a la de `Librerías` (*Libraries*, en el inglés) y en los botones de más allá a la derecha, le damos al que dice `Añadir JARs externos...` (*Add External JARs...*, en el inglés) y seleccionamos el `JDA-4.0.0_39-withDependencies.jar`.
> Hasta este momento, ya tenemos el JAR instalado en Eclipse manualmente pero si queremos, instalamos los *Sources* y *Javadocs* con este paso #3 (Muy recomendado).

- **Configurando Soruces y Javadocs de JDA**.
En la misma lista de las `Librerías` expandimos el JAR que agregamos posteriormente (`JDA-4.0.0_39-withDependencies.jar`) y seleccionamos `Archivo fuente adjunto` (*Source attachment*, en el inglés) y damos clic en uno de los botones que dice `Editar` (*Edit*, en el inglés), de la derecha y damos clic en la opción de `Ubicación externa` (*External Location*, en el inglés) y elejimos el archivo que descargamos de los *Sources* llamado `JDA-4.0.0_39-sources.jar`.
Realizamos lo mismo pero con el `Ubicación del Javadoc` (*Javadoc Location*, en el inglés) y su archivo `JDA-4.0.0_39-javadoc.jar`, es decir, damos clic en `Editar`, seguido de eso, damos clic en la opción de `Ubicación externa` y elejimos el archivo de los *Javadoc* llamado `JDA-4.0.0_39-javadoc.jar`

- **¿Dos, tres?** Sí... Creo que lo viste muy bien, son el número de pasos para instalar JDA manualmente utilizando JARs con o sin las anotaciones y el autocompletar en Eclipse. **Puedes seguir con la guía dándole [clic aquí](#primer-inicio)**.

### NetBeans JAR
- **Creamos el proyecto**.
Una vez descargados los JARs de JDA, podemos abrir el NetBeans y hacer una nueva aplicación de Java normal, dándole a *Java* en la lista de categorías y `Aplicación de Java` (o también, *Java Application*, en inglés).

- **Añadir el JAR de JDA**.
Seguido de crear el proyecto, damos clic derecho a la carpeta `Librerías` (o, *Libraries*, en inglés) y seleccionamos la opción `Añadir JAR/Carpeta...` (o, *Add JAR/Folder...*, en inglés), ahí buscamos el `JDA-4.0.0_39-withDependencies.jar` y lo añadimos.
> Hasta ese último punto, está todo bien, todo correcto pero faltan algunas cosas, como las anotaciones y el autocompletar, si queremos añadirlos para no tener que escribir todo eso, simplemente hacemos el paso #3 (Demasiado recomendado).

- **Configurar Sources y Javadocs de JDA**.
En la carpeta de las `Librerías`, seleccionamos la librería de JDA que acabamos de agregar y presionamos clic derecho encima de ella, y seleccionamos `Editar...`, damos a `Explorar...` (o, *Browse...*, en inglés) en el `Javadoc` y seleccionamos `JDA-4.0.0_39-javadoc.jar`. Lo mismo realizamos pero con el de abajo (`Sources`) y seleccionamos `JDA-4.0.0_39-sources.jar`

- **Y los 4, 5, 6, 7, 8, 9, 10, pasos restantes?** No hay, ya completaste la total instalación de JDA con JARs con o sin las anotaciones en NetBeans. **Puedes seguir con la guía dándole [clic aquí](#primer-inicio)**.

### IntelliJ IDEA JAR
1. **Creamos el proyecto**.
Le damos a `Crear nuevo proyecto` (*Create New Project*, inglés) o vamos a `Archivo > Nuevo > Proyecto` (*File > New > Project*, inglés) y en la lista de la izquierda seleccionamos *Java*, una vez seleccionado no tocamos más nada que no sea el SDK del proyecto, que debe ser superior a Java 8, seguido presionamos `Siguiente` (*Next*, inglés), si queremos hacer nuestro proyecto con alguna base o plantilla seleccionamos una y le damos `Siguiente`. Colocamos algún nombre al proyecto y su ubicación en el disco duro o disco extraíble. Presionamos `Finalizar` (*Finish*, inglés).

2. **Añadir el JAR de JDA**.
Debemos ir a `Archivo > Estructura de proyecto` (*File > Project Structure*, inglés), en esa ventana que nos aparece seleccionamos el menú de la izquierda llamado `Módulos` (*Modules*, inglés) y seleccionamos el primero o el que aparece ya en la lista.
En el cuadrado de la derecha vamos a la pestaña `Dependencias` (*Dependencies*, inglés) que se ubica por debajo del nombre del módulo. Ahora le damos al signo de más (+) que se ubica a la derecha del todo en el recuadro, presionamos `JARs o directorios...` (*JARs or directories...*, inglés) ahí buscamos la ubicación del `JDA-4.0.0_39-withDependencies.jar` y presionamos `OK`.
> Listooo, ya tenemos el JAR de JDA instalado, con IntelliJ se puede decompilar el JAR y obtener sus funciones. Aunque podemos completar y dejar de utilizar el decompilador por cuestiones de las licencias, podemos realizar el paso #3.

3. **Configurar Sources y Javadocs de JDA**.
En la pestaña de `Dependencias` buscamos el JDA y presionamos clic derecho encima de él, seguido de eso en el menú contextual damos clic en `Editar` (*Edit*, inglés), damos clic al primer símbolo de añadir o de más (+) que no tiene ningún dibujo o gráfico adicional, y seleccionamos el archivo del `JDA-4.0.0_39-sources.jar` y después vamos otra vez al símbolo y presionamos el `JDA-4.0.0_39-javadoc.jar`.

- **¡Felicidades!** Haz completado la instalación de JDA manual con los JARs descargados y con el autocompletar del IntelliJ o del *Sources* y *Javadoc*. **Puedes seguir con la guía dándole [clic aquí](#primer-inicio)**.

---

## Primer inicio
Para empezar con el primer inicio del bot con JDA, ya deberíamos tener el JDA instalado en nuestro proyecto a través de cualquiera de los métodos de instalación anteriormente leídos o escritos. Y empezamos con algo básico, tener nuestro bot en línea.
- Creamos un `package` dentro de la carpeta en la que se encontrará todo nuestro código y dará un mayor orden (opcional).
- Creamos una clase con el nombre de nuestro bot o `Main`, la cual será la primera a ejecutar y si estamos dentro de un *package* se especifica en la primera línea utilizando (a pesar de que algunos IDEs ya traen esta línea por defecto al crear la clase):

```java
package algo.algo.algo;
```

- Empezamos importando todas las clases del *package* llamado `api` de `net.dv8tion.jda`, para esto utilizamos la línea de código, ahí el `*` indica que son todos los archivos dentro de ese *package*:

```java
import net.dv8tion.jda.api.*;
```

- Proseguimos con la creación de la clase (en algunos IDEs se crea automáticamente con el nombre del acrhivo o clase que definimos anteriormente), para esto utilizamos las siguientes líneas de código, la cual define una clase pública llamada `Main` o como le colocamos al principio y dentro de toda esa clase, es decir, dentro de los corchetes (*{ }*) colocamos todo el código que dará la clase, pueden ser funciones o variables, públicas o privadas, estáticas:

```java
public class Main {

}
```

- Se crea la variable pública, estática y sin un valor de inicio pero que se dará más adelante, será un tipo de variable `JDA` y tendrá un nombre de `bot` aunque puede ser reemplazado por cualquier otro que podamos recordar muy fácilmente, por ejemplo, el nombre de nuestro bot y para eso, utlizamos la siguiente línea de código dentro de la clase:

```java
public static JDA bot;
```

- Empieza la función principal o `main`, esta función será pública, estática y no devolverá nada, tendrá un parámetro de un conjunto de strings llamado `args`, también lanzará una excepción que probablemente nos dé JDA al iniciar el bot por si el token está mal o cualquier otro error, para definir esa función se utiliza (dentro de la clase principal y debajo de la variable `bot`), aquí empezará nuestro código a ejecutarse:

```java
public static void main(String[] args) {

}
```

- Dentro de esta función, le damos valor a la variable `bot` que será el resultado de la construcción de una cuenta de Discord tipo BOT y se define su token, esto lo hacemos por medio de estas tres líneas de código:

```java
bot = new JDABuilder(AccountType.BOT)
    .setToken("Token")
    .build();
```

- Y listo, al ejecutar todas esas líneas de código obtendremos a nuestro bot en línea utilizando la librería JDA con el lenguaje de programación *Java*, en resultado a todo lo anterior escrito y brevemente explicado, quedaría así:

```java
package ejem.plo; // Definimos si está dentro de un package y cuál es ese package

import net.dv8tion.jda.api.*; // Importamos todos los archivos de la librería de net.dv8tion.jda.api

public class Main { // Reemplazar Main con el nombre del archivo
    public static JDA bot; // Este será la instancia del bot, puede ser bot reemplazado por el nombre de preferencia de ustedes
    
    public static void main(String[] args) throws Exception { // Se empieza con la función main o principal,
        // Se dice que arroja un error o excepción en esta función,
        // Todo lo primero a ejecutar va aquí:
        bot = new JDABuilder(AccountType.BOT) // Utilizamos un constructor de JDA llamado JDABuilder y se define el tipo de cuenta, en este caso BOT,
            .setToken("Token del bot") // Se define el token de la cuenta del bot a usar y por último,
            .build(); // Se "construye" el bot, esto será lo almacenado en la variable y este, intentará conectarse a Discord
    }
}
```

---

## Conclusión
Hemos podido instalar *Java* en una versión superior o igual a la `8` y también, JDA en su versión `4.0.0_39` utilizando alguno de los tres métodos diferentes (*Maven*, *Gradle* y/o *JARs*) en tres diferentes IDEs que existen o son los más populares para programar en *Java*.
También hemos aprendido como encender nuestro bot o colocarlo en línea por primera vez utilizando JDA.

<font size=4>**¡Muchas gracias por leer! Continúa con como manejar todos los datos que recibe el bot, por medio de eventos, [aquí](/java/eventos.md)**</font>
