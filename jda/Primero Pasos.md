# Índice

1. [Introducción](##introduccion) - Breve explicación sobre JDA.
2. [Instalación](##instalacion) - Coḿo instalar y/o usar JDA en nuestro sistema.

## Introducción

[JDA](https://github.com/DV8FromTheWorld/JDA "JDA GitHub") *(Java Discord API)* es un API Wrapper para [Discord API](https://discordapp.com/developers/docs/intro "Discord API Documentation") que facilita enormemente su uso, es decir, una herramienta para hacer más accesible la API de Discord.

**JDA** fue desarrollado por [Austin Keener](https://github.com/DV8FromTheWorld/ "Developer Home") sirviendo como librería para la creación, manejo y modificación de Bots en Java.

## Instalación

[![JDK](https://img.shields.io/badge/JDK-8+-red.svg)](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html "JDK 8 Download")
[![Maven](https://img.shields.io/badge/Maven-Lastest-yellow.svg)](https://maven.apache.org/ "Maven Download")
[![JDA Wiki](https://img.shields.io/badge/Wiki-Home-blue.svg)](https://github.com/DV8FromTheWorld/JDA/wiki "JDA Wiki")

- [Requisitos](###requisitos)
- [Instalación con JAR](###version-jar)
- [Instalación con Maven](###version-maven)

---

### Requisitos

Para hacer uso de la librería **JDA** necesitaremos tener [JDK 8](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html "JDK 8 Download") o superior instalado en nuestro Sistema.

Podemos comprobar si tenemos **JDK 8** instalando escribiendo `java -version` en la consola de nuestro Sistema Operativo.

Aunque se puede compilar y ejecutar por consola, en esta explicación usaremos un IDE (Entorno de Desarrollo) para facilitarnos el proceso de usar librerías externas, y se explicará tanto el uso directo de dependencias (archivo JAR), como el uso de proyectos Maven.

Se hará un explicación para los IDEs: *NetBeans*, *Eclipse* e *IntelliJ IDEA*.

#### Links de Descarga

- [JDK 8 (Java Development Kit)](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html "JDK 8 Download")
- [Archivos JAR de JDA](https://github.com/DV8FromTheWorld/JDA/releases/ "Archivos JAR")
- [NetBeans IDE](https://netbeans.org/ "NetBeans Homepage")
- [Eclipse](https://www.eclipse.org/ide/ "Eclipse Homepage")
- [IntelliJ IDEA](https://www.jetbrains.com/idea/ "IntelliJ IDEA Homepage")

---

### Versión Maven

El sistema de versiones y dependencias de Maven nos permite usar librerías externas sin necesidad de descargar el JAR de estas, con la simple modificación del archivo `pom.xml` propio de los proyectos Maven.

Partiendo de que un documento `pom.xml` base sería así:

```xml
<!--Etiqueta project, groupId, name y otras cosas-->
<properties>
    <!--Más etiquetas con propiedades-->
</properties>

<!--Aquí debería estar la etiqueta <dependencies>, si no está simplemente la creamos.-->
<dependencies>
    <!--Si ya existía previamente la etiqueta, simplemente añadimos las <dependency>-->
    <!--JUnit es necesario-->
    <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>3.8.1</version>
      <scope>test</scope>
    </dependency>
    <!--Añadimos el JDA-->
    <dependency>
        <groupId>net.dv8tion</groupId>
        <artifactId>JDA</artifactId>
        <version>3.6.0_362</version>
    </dependency>
</dependencies>

<!--De igual manera que con <dependencies>, si no está <repositories>, se crea-->
<repositories>
    <!--Añadimos lo siguiente-->
    <repository>
        <id>jcenter</id>
        <name>jcenter-bintray</name>
        <url>http://jcenter.bintray.com</url>
    </repository>
</repositories>
```

- [NetBeans](####netbeans-maven)
- [Eclipse](####eclipse-maven)
- [IntelliJ](####intellij-idea-maven)

#### NetBeans Maven

#### Eclipse Maven

#### IntelliJ IDEA Maven

---

### Versión JAR

Este documento está **incompleto**, intentaremos tenerlo listo lo antes posible. Perdón por las molestias.
