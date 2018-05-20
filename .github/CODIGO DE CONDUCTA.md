# CÓDIGO DE CONDUCTA
Este repositorio sigue las siguientes normalizaciones para proporcionar un flujo de trabajo correcto en equipo.

**NOTA:** Por favor, si eres contribuidor de este repositorio o estás interesado en contribuir, sigue estas normas.

* **Tabla de contenido**
  * [Documentación](#documentación)
    * [Sobre el orden de la documentación de archivos](#Sobre-el-orden-de-documentación-de-archivos)
  * [Códigos públicos](#códigos-públicos)
    * [Sobre la públicación de códigos públicos]()

# Documentación

![](./assets/logo-docs.png)

## Sobre el orden de documentación de archivos

La estructura de la documentación debe seguir estrictamente un `CÓDIGO DE CONDUCTA` para que los commits sean ordenados y se pueda trabajar colectivamente bien.

### Esquema de archivos

Dentro de la rama /docs/
```
.
└── /js
    ├── discord-js.md
    ├── Primeros Pasos.md
    └── [md extras].md
└── /py
    ├── discord-py.md
    ├── Primeros Pasos.md
    └── [md extras].md 
└── /[iniciales del lenguaje de programación]
    ├── [librería].md
    ├── Primeros Pasos.md
    └── [md extras].md
```

### Esquema de categorización en el sidebar
Editando (Ejemplo) `_sidebar.md`:
```
* [Índice](/)

* JavaScript
  * **discord.js**
    * [¿Qué es discord.js?](/js/discord-js.md)
    * [Primeros Pasos](/js/Primeros-Pasos.md)
    * [Snippets](/js/Snippets.md)

* Python
  * **discord.py**
    * [¿Qué es discord.py?](/py/discord-py.md)
    * [Primeros Pasos](/py/Primeros-Pasos.md)

* [Lenguaje de programación]
  * [**Nombre de la librería**]
     * [¿Qué es [librería]?](/[iniciales del lenguaje de programación]/[libreria].md)
     * [Primeros Pasos](/[iniciales del lenguaje de programación]/Primeros Pasos.md)
```

# Códigos públicos

![](./assets/logo-code.png)

## Sobre la públicación de códigos públicos

La estructura de publicación de códigos públicos (snippets - códigos reciclables) debe seguir el siguiente orden de ubicación:

### Esquema de archivos
Dentro de la rama /code/:
```
.
└── /js
    └── /discord-js
        └── /[usuario/autor]
            └── archivo.js
```