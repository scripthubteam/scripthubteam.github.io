

# Objetos: Propiedades y métodos
En la vida real un camión es un objeto. Este tiene propiedades:

`camion.color = "rojo"
camion.marca = "fiat"
`

y métodos:

`camion.arrancar()
camion.parar()
`
## Propiedades
Ahora escribiremos las `propiedades` del camión en JavaScript y le asignaremos `valores`.

```js
var camion = {
  color: "rojo",
  antiguedad: 50
}
```

Ya definimos que mi camión es rojo y tiene una antiguedad de 50 años. Pero ahora, ¿cómo accedemos a esos datos?
Existen dos formas:

```js
camion.color //Retornará rojo
camion["color"] //Retornará rojo
```

## Métodos
Los métodos son acciones que pueden funcionar como objetos. Se definen de la siguiente manera:

```js
var camion = {
  color: "rojo",
  antiguedad: 50
  colorAntiguedad: function() {
      return this.color + " " + this.antiguedad;
    }
  }
```
Y accedemos de la siguiente forma:

```js
var fnc = camion.colorAntiguedad()
```
