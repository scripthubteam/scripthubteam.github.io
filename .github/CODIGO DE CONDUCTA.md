# CÓDIGO DE CONDUCTA
Este repositorio sigue las siguientes normalizaciones para proporcionar un flujo de trabajo correcto en equipo.

**NOTA:** Por favor, si eres contribuidor de este repositorio o estás interesado en contribuir, sigue estas normas.

* **Tabla de contenido**
  * [Sobre el orden de la documentación de archivos](#Sobre-el-orden-de-documentación-de-archivos)

# Sobre el orden de documentación de archivos
La estructura de la documentación debe seguir estrictamente un `CÓDIGO DE CONDUCTA` para que los commits sean ordenados y se pueda trabajar colectivamente bien.

### Esquema de archivos
```
.
└── js
    ├── discord-js.md
    └── archivos-extra.md
└── py
    ├── discord-py.md
    └── archivos-extra.md 
└── [iniciales del lenguaje de programación]
    ├── [librería].md
    └── [md extras (opcional)].md
```

### Esquema de categorización en el sidebar
Editando `_sidebar.md`:
```
* [Índice](/)

* JavaScript
  * [discord.js](/js/discord-js.md)

* Python
  * [discord.py](/py/discord-py.md)

* [Lenguaje de programación]
  * [Nombre de la librería](/[iniciales del lenguaje de programación]/[librería].md)
     * [Extra](/[iniciales del lenguaje de programación]/[md extras].md)
```
