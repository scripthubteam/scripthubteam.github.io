
### Lo básico pero indispensable
## Console.log()
Comenzaremos viendo la función `console.log()`, la cual nos permitirá "imprimir" un mensaje en nuestra consola.

```js
console.log("Hola")
```

Esto, como es de esperar, imprimiría "Hola" en nuestra consola.

## Comentarios
En JavaScript podemos comentar nuestro código escribiendo `// (para una línea)` o `/**/ (para un bloque de líneas/comentario)`.
El programa ignorará por completo lo escrito entre esos signos.

```js
console.log("Hola") //Imprime Hola en la consola

// Esto es un comentario de una línea.
/*
Esto es un comentario bloque de comentario.
*/
```

## Variables
En JavaScript una variable es un contenedor para guardar distintos tipos de datos.

```js
var x = 10
var myString = "Hola"

console.log(x)
console.log(myString)
```

Pueden concatenarse con una string mediante el signo `+` acompañado de `""`.

```js
var nombre = "Lau"
console.log("Mi nombre es "+nombre)
```

## Datos variados
* **JavaScript es sensible al caso.**
  
  No es lo mismo escribir `edad` que `Edad`.
  ```js
  var edad = 20 //Una Variable
  var Edad = 20 //Otra variable
  ```
* **Bloque de código.**

  Son un conjunto de enunciados que deberán ejecutarse con una secuencia completa. Se indican con `{}`.
* **Aprendamos a contar desde 0**

  Algo muy importante que debes aprender es que JavaScript cuenta desde `0`.
  
  ```js
  var cuenta = "Hola que tal"
  ```
  En este caso:
  `Hola` equivale a la posición 0, `que` a la primera y `tal` a la segunda.
  
**¡Muy bien!**, ahora que sabemos lo básico, podemos continuar hacia los [Tipos de datos](/js/datatyp.md) 
