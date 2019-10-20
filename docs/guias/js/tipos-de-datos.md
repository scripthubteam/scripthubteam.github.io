

# Tipos de dato
En JavaScript existen 7 tipos de dato según la última definición estándar de ECMAScript:

* String
* Número
* Boolean 
* Objeto
* Null 
* Undefined 
* Símbolo *(No tratado en la guía)*

## Strings

Un string es un texto. Debe ser escrito entre `""` o `''`.

```js
"String"
'String'
```

## Números
JavaScript solo tiene un tipo de números.
Pueden ser escritos con decimal, sin decimal o en notación científica.

```js
100.00 //con decimal
100 //sin decimal
123e5 //12300000
```

## Booleans
Un booleano puede tener dos valores: `true` o `false`.

```js
var u = 5
var x = 5
var b = "Hola"
(u === x) //Retorna true
(u === b) //retorna false
```

## Arrays
Un array es una lista. Los datos se separan por coma.

```js
var myArray = ["Yamaha", "Fiat", "Nissan"]
```

## Objetos
Un objeto es un entidad independiente con propiedades y tipos.

```js
var miAuto = {
  color: "rojo",
  marca: "fiat"
}

console.log("Mi auto es color "+miAuto.color)
```

## Undefined
En JavaScript una variable sin un valor asignado, tendrá un valor `undefined`.

```js
var hola; //Valor indefinido, tipo undefined.
```

## Null
En JavaScript `null` es nada, algo inexistente, aunque es tomado como un tipo de objeto.

```js
var miAuto = {
  color: "rojo",
  marca: "fiat"
}
miAuto = null //El valor ahora es nulo, pero seguirá siendo un objeto.
```
