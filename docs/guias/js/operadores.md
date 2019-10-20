

# Operadores
Los operadores son símbolos que permiten realizar distintas operaciones. Hay muchos tipos de operadores, pero voy a explicar 3 de ellos.

  *  Operadores aritméticos: Se emplean para realizar operaciones aritméticas con valores y/o variables.
  *  Operadores comparativos: Se emplean para establecer comparaciones entre valores y/o variables.
  *  Operadores lógicos: Se emplean para establecer condiciones.

## Aritméticos

Para sumar utilizamos el operador `+`.

```js
var n1 = 10
var n2 = 9
var total = n1 + n2
//Output: 19
```

Para restar se utiliza el operador `-`.

```js
var n1 = 10
var n2 = 9
var total = n1 - n2
//Output: 1
```

Para multiplicar se utiliza el operador `*`.

```js
var n1 = 10
var n2 = 9
var total = n1 * n2
//Output: 90
```

Para dividir se utiliza el operador `/`. El resultado será el cociente de la división.

```js
var n1 = 10
var n2 = 5
var total = n1 / n2
//Output: 2
```

Para obtener el resto de una división utilizaremos el operador `%`.

```js
var n1 = 10
var n2 = 5
var total = n1 % n2
//Output: 0
```

## Operadores comparativos

Con el operador `>` indicamos si x dato es `mayor que` z dato.

```js
4 > 3 //true
4 > 5 //false
```

Con el operador `<` indicamos si x dato es `menor que` z dato.

```js
4 < 3 //false
4 < 5 //true
```

Con el operador `==` comprobaremos si x dato es `igual` a z dato.

```js
4 == 4 //true
4 == 5 //false
```

Con el operador `!=` comprobaremos si x dato es `distinto` de z dato.

```js
4 != 4 //false
4 != 5 //true
```

Con el operador `>=` comprobaremos si x dato es `mayor que` z dato.

```js
4 >= 4 //true
4 >= 3 //false
```

Con el operador `<=` comprobaremos si x dato es `menor que` z dato.

```js
4 <= 4 //true
4 <= 5 //false
```

Con el operador `===` comprobaremos si x dato es `idéntico` a z dato.
Este operador es similar a `==`, pero no igual.
Chequea si los datos ingresados son iguales y si son del mismo tipo de variable.

```js
var a = 15
var b = "15"
var c = 15
a === b //false
a === c //true
```

Con el operador `===` comprobaremos si x dato `no es idéntico` a z dato.
Este operador es similar a `!=`, pero no igual.
Chequea si los datos ingresados no son iguales o si no son del mismo tipo de variable.

```js
var a = 15
var b = "15"
var c = 15
a !== b //true
a !== c //false
```

Los **operadores lógicos** serán explicado junto a las [condiciones](/js/cond.md).
