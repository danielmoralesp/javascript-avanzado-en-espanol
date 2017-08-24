# 1.11 Scope \(Ámbito\)

Si le pregunta al empleado de la tienda de teléfonos por un modelo de teléfono que su tienda no tiene, no podrá venderle el teléfono que desea. Sólo tiene acceso a los teléfonos en el inventario de su tienda. Tendrás que buscar otra tienda para ver si puedes encontrar el teléfono que estás buscando.

La programación tiene un término para este concepto: scope \(técnicamente llamado ámbito léxico\). En JavaScript, cada función obtiene su propio ámbito. El scope es básicamente una colección de variables, así como las reglas de cómo se accede a esas variables por nombre. Sólo el código dentro de esa función puede acceder a las variables de ámbito de la función.

Un nombre de variable tiene que ser único dentro del mismo ámbito - no puede haber dos variables diferentes una al lado del otro. Pero el mismo nombre de variable podría aparecer en diferentes ámbitos.

```js
function one() {
	// ésta `a` solo pertenece a la funcion `one()`
	var a = 1;
	console.log( a );
}

function two() {
	// ésta `a` solo pertenece a la funcion `two()` 
	var a = 2;
	console.log( a );
}

one();		// 1
two();		// 2
```

También, un scope puede estar anidado dentro de otro scope, apenas como si un payaso en una fiesta del cumpleaños sopla encima de un globo dentro de otro globo. Si un scope está anidado dentro de otro, el código dentro del ámbito más interno puede acceder a las variables desde cualquiera de los ámbitos.

Considere:

```js
function outer() {
	var a = 1;

	function inner() {
		var b = 2;

		// puede acceder a ambos `a` y `b` aqui
		console.log( a + b );	// 3
	}

	inner();

	// solo puede acceder a `a` aqui
	console.log( a );			// 1
}

outer();
```

Las reglas de ámbito léxico dicen que el código en un ámbito puede acceder a la variable `a`  de ese ámbito o de cualquier ámbito fuera de él.

Por lo tanto, el código dentro de la función `inner()` tiene acceso a las variables `a` y `b`, pero el código en `outer()` sólo tiene acceso a `a` - no puede acceder a `b` porque esa variable sólo está dentro de `inner()`.

Recuerde este fragmento de código anterior:

```js
const TAX_RATE = 0.08;

function calculateFinalPurchaseAmount(amt) {
	// calculate the new amount with the tax
	amt = amt + (amt * TAX_RATE);

	// return the new amount
	return amt;
}
```

La constante `TAX_RATE` \(variable\) es accesible desde dentro de la función `calculateFinalPurchaseAmount(..)`, aunque no la pasamos, debido al alcance léxico.

Nota: Para obtener más información sobre el ámbito léxico, consulte los tres primeros capítulos del título Scope & Closures de esta serie.

