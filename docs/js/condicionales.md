

# Condicionales
En alguna parte de nuestro script necesitaremos comprobar que si x condición se cumple, se ejecuta un código o no. Para ello existen las estructuras condicionales.

## if y else
Al utilizar `if` podemos comprobar que sí una condición se cumple, se ejecute una parte del código. Usando `else` podemos establecer una alternativa, aunque es opcional.

```js
if (condicion) {
  //código a ejecutarse si se cumple la condición
} else {
  //código a ejecutarse si no se cumple la condición
}
```

Por ejemplo, comprobaremos si mi edad es mayor a 10.

```js
var miEdad = 20
if (miEdad > 10) {
  console.log("Yay")
} else {
  console.log(":(")
}
//Output: Yay
```

Si tenemos más de dos condiciones podemos utilizar `else if`.

```js
var miEdad = 20
if (miEdad == 10) {
  console.log("Yay")
} else if (miEdad > 19) {
  console.log(":)")
} else {
  console.log(":(")
}
```

## Operadores lógicos

Teníamos pendiente explicar este tipo de operadores que se utilizan para ampliar las posibilidades de establecer condiciones.

Con el operador `!` negaremos una condición.

```js
if (!miEdad) {
  console.log("La variable miEdad no está declarada.")
}
```

Con el operador `||` ejecutaremos el bloque de código si se cumple alguna de las condiciones dadas. `||` puede traducirse al español como `o`.

```js
var miEdad = 20
var miAuto = "rojo"
if (miEdad == 20 || miAuto === "rojo") {
  //código a ejecutar si miEdad es igual a 20 y miAuto es idéntico a rojo, en este caso, este bloque de código sería ejecutado
} else {
  //codigo a ejecutar si no se cumple ninguna condición
}
```

Con el operador `&&` ejecutaremos el bloque de código si se cumplen las dos condiciones. `&&` puede traducirse al español como `y`.

```js
var miEdad = 20
var miAuto = "azul"
if (miEdad == 20 && miAuto === "rojo") {
  //código a ejecutar si se cumplen las dos condiciones
} else {
  //código a ejecutar si alguna de las condiciones no se cumplen. En este caso miAuto no es rojo, sino azul, así que este bloque de código se ejecutaría
}
```
