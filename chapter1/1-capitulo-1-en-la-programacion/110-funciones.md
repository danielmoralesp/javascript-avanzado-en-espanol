# 1.10 Funciones

El empleado de la tienda de teléfonos probablemente no lleva consigo una calculadora para calcular los impuestos y la cantidad final de la compra. Esa es una tarea que necesita definir una vez y reutilizar una y otra vez. Las probabilidades son, que la empresa tenga un registro de pago \(computadora, tableta, etc.\) con esas "funciones" incorporadas.

Del mismo modo, su programa casi seguramente querrá dividir las tareas del código en piezas reutilizables, en lugar de repetir repetidamente repetidamente \(juego de palabras!\). La forma de hacerlo es definir una función.

Una función es generalmente una sección con el nombre del código que puede ser "llamada" por ese nombre, y el código dentro de ella se ejecutará cada vez que se llama. Considere:

```js
function printAmount() {
	console.log( amount.toFixed( 2 ) );
}

var amount = 99.99;

printAmount(); // "99.99"

amount = amount * 2;

printAmount(); // "199.98"
```

Opcionalmente, las funciones pueden tomar argumentos \(también conocidos como parámetros\), valores que se le "pasan". Además, también pueden devolver un valor.

```js
function printAmount(amt) {
	console.log( amt.toFixed( 2 ) );
}

function formatAmount() {
	return "$" + amount.toFixed( 2 );
}

var amount = 99.99;

printAmount( amount * 2 );		// "199.98"

amount = formatAmount();
console.log( amount );			// "$99.99"
```

La función `printAmount(..)` toma un parámetro que llamamos `amt`. La función `formatAmount()` devuelve un valor. Por supuesto, también puedes combinar esas dos técnicas en la misma función.

Las funciones se usan a menudo para el código que se planea llamar varias veces, pero también pueden ser útiles sólo para organizar fragmentos de código relacionados en las colecciones con nombre, incluso si sólo planea llamarlas una vez.

Considere:

```js
const TAX_RATE = 0.08;

function calculateFinalPurchaseAmount(amt) {
	// calculate the new amount with the tax
	amt = amt + (amt * TAX_RATE);

	// return the new amount
	return amt;
}

var amount = 99.99;

amount = calculateFinalPurchaseAmount( amount );

console.log( amount.toFixed( 2 ) );		// "107.99"
```

Aunque `calculateFinalPurchaseAmount(..)` sólo se llama una vez, organizar su comportamiento en una función denominada separadamente hace que el código que utiliza su lógica \(la declaración `amount = calculateFinal...` \) más limpia. Si la función tenía más declaraciones en ella, los beneficios serían aún más pronunciados.



