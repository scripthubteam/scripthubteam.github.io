# Índice

1. [Introducción](#introducción)
2. [Requisitos](#requisitos)

# Introducción

![Dificultad](https://img.shields.io/badge/Nivel-Intermedio%2FAvanzado-orange.svg)

Nuestro objetivo es crear un sistema que sea capaz de reconocer cualquier Clase que introduzcamos en un paquete, al que llamaremos *plugins*, como un plugin que podrá ser llamado por un comando con su mismo nombre.

De esta manera, implementar funciones a nuestro Bot será mucho más fácil ya que cada función será una Clase en sí misma.

__Ejemplo:__

![Ejemplo 1](https://i.imgur.com/4mYy8QL.png)
![Ejemplo 1](https://i.imgur.com/vkDj4ZQ.png)

# Requisitos

[![JDK](https://img.shields.io/badge/JDK-8+-red.svg)](http://www.oracle.com/technetwork/java/javase/downloads/jdk8-downloads-2133151.html "JDK Download")
[![Reflections](https://img.shields.io/badge/Reflections-Lastest-blue.svg)](https://github.com/ronmamo/reflections "Reflections GitHub")

Para este sistema vamos a necesitar la dependencia [Reflections](https://github.com/ronmamo/reflections) que puede ser fácilmente añadida a un proyecto Maven añadiendo lo siguiente en la etiqueta `<dependencies>`:

```xml
<dependency>
    <groupId>org.reflections</groupId>
    <artifactId>reflections</artifactId>
    <version>0.9.11</version>
</dependency>
```

Aunque desarrollaremos el sistema con JDK 8, este puede ser integrado en cualquier Bot escrito con un JDK superior.