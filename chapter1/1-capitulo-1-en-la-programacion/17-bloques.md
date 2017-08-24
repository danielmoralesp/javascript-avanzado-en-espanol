# 1.7 Bloques

El cliente de la tienda telefónica debe pasar por una serie de pasos para completar la compra mientras compra su nuevo teléfono.

Del mismo modo, en código a menudo es necesario agrupar una serie de declaraciones en conjunto, que a menudo llamamos un bloque. En JavaScript, un bloque se define envolviendo una o más instrucciones dentro de un par de llaves `{..}`. Considere:

```js
var amount = 99.99;

// un bloque general
{
	amount = amount * 2;
	console.log( amount );	// 199.98
}
```

Este tipo de bloque independiente general `{..}`  es válido, pero no es tan comúnmente visto en los programas de JS. Por lo general, los bloques se adjuntan a alguna otra instrucción de control, como una instrucción `if` \(consulte "Condicionales"\) o un bucle \(consulte "Loops"\). Por ejemplo:

```js
var amount = 99.99;

// is amount big enough?
if (amount > 10) {			// <-- block attached to `if`
	amount = amount * 2;
	console.log( amount );	// 199.98
}
```

Vamos a explicar las declaraciones `if` en la siguiente sección, pero como puede ver, el bloque `{..}` con sus dos declaraciones se adjunta a `if (amount > 10)`; Las declaraciones dentro del bloque sólo se procesarán si pasa el condicional.

Nota: A diferencia de la mayoría de otras sentencias como `console.log(amount);`, una instrucción de bloque no necesita un punto y coma \(;\) para concluirlo.



