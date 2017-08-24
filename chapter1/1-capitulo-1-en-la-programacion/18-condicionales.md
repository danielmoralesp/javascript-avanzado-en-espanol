# 1.8 Condicionales

"¿Desea agregar los protectores de pantalla adicionales a su compra, por $ 9.99?" El empleado de la tienda telefónica le ha pedido que tome una decisión. Y puede que necesite consultar primero el estado actual de su cartera o cuenta bancaria para responder a esa pregunta. Pero, obviamente, esto es sólo una simple pregunta "sí o no".

Hay bastantes maneras de expresar condicionales \(aka decisiones\) en nuestros programas.

La más común es la instrucción `if`. Esencialmente, usted está diciendo: "Si esta condición es cierta, haga lo siguiente ...". Por ejemplo:

```js
var bank_balance = 302.13;
var amount = 99.99;

if (amount < bank_balance) {
	console.log( "I want to buy this phone!" );
}
```

La instrucción `if` requiere una expresión entre los paréntesis `()` que puede tratarse como verdadero o falso. En este programa, hemos proporcionado la expresión `amount < bank_balance`, que de hecho se evaluará a `true` o `false` dependiendo de la cantidad en la variable `bank_balance`.

Incluso puede proporcionar una alternativa si la condición no es verdadera, llamada cláusula `else`. Considere:

```js
const ACCESSORY_PRICE = 9.99;

var bank_balance = 302.13;
var amount = 99.99;

amount = amount * 2;

// can we afford the extra purchase?
if ( amount < bank_balance ) {
	console.log( "I'll take the accessory!" );
	amount = amount + ACCESSORY_PRICE;
}
// otherwise:
else {
	console.log( "No, thanks." );
}
```

Aquí, si `amount < bank_balance` es `true`, imprimiremos "¡Tomaré el accesorio!" Y agregue 9.99 a nuestra variable `amount`. De lo contrario, la cláusula `else` dice que simplemente responderemos cortésmente con "No, gracias". Y dejar la cantidad sin cambios.

Como discutimos en "Valores y Tipos" anteriormente, los valores que no son ya de un tipo esperado son a menudo coaccionados a ese tipo. La sentencia `if` espera un booleano, pero si se pasa algo que no sea ya booleano, la coerción se producirá.

JavaScript define una lista de valores específicos que se consideran "falseados" porque cuando se coaccionan a un booleano, se convierten en falsos - estos incluyen valores como `0` y `""`. Cualquier otro valor que no esté en la lista de "falsos" es automáticamente "verdad" - cuando se coacciona a un booleano se convierten en verdaderos. Los valores Truthy incluyen cosas como 99.99 y "free". Vea "Truthy & Falsy" en el Capítulo 2 para más información.

Los condicionales existen en otras formas además del `if`. Por ejemplo, la instrucción `switch` puede usarse como una abreviatura para una serie de instrucciones `if..else` \(vea el Capítulo 2\). Los bucles \(ver "Loops"\) usan un condicional para determinar si el bucle debe continuar o detenerse.

Nota: Para obtener información más detallada sobre las coerciones que pueden ocurrir implícitamente en las expresiones de prueba de condicionales, consulte el Capítulo 4 del título de Tipos y Gramática de esta serie.



