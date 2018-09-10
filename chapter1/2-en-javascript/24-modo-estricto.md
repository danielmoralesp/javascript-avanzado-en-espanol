# 2.4 Modo estricto

ES5 agregó un "modo estricto" al lenguaje, que aprieta las reglas para ciertos comportamientos. Generalmente, estas restricciones se consideran como mantener el código a un conjunto de directrices más seguro y más apropiado. Además, la adhesión al modo estricto hace que su código en general sea más optimizable por el motor. El modo estricto es un gran triunfo para el código, y deberías usarlo para todos tus programas.

Puede optar por el modo estricto para una función individual, o un archivo entero, dependiendo de donde se puso el modo estricto:

```js
function foo() {
	"use strict";

	// este código esta en modo estricto

	function bar() {
		// este código esta en modo estricto
	}
}

// este código no esta en modo estricto
```

Compare esto con:

```js
"use strict";

function foo() {
	// este código esta en modo estricto

	function bar() {
		// este código esta en modo estricto
	}
}

// este código esta en modo estricto
```

Una diferencia clave \(¡mejora!\) con el modo estricto es rechazar la declaración implícita de variables globales automáticas al omitir la variable:

```js
function foo() {
	"use strict";	// activa el modo etsricto
	a = 1;		// `var` olvidada, es igual a ReferenceError
}

foo();
```

Si activa el modo estricto en su código, y obtiene errores, o el código comienza a comportarse con errores, su tentación podría ser evitar el modo estricto. Pero ese instinto sería una mala idea para complacerse. Si el modo estricto causa problemas en su programa, casi seguro que es una señal de que tiene cosas en su programa que debe arreglar.

No sólo el modo estricto mantendrá su código en un camino más seguro, y no sólo hará que su código sea más optimizable, sino que también representa la dirección futura del lenguaje. Sería más fácil para ti acostumbrarte al modo estricto ahora que seguir poniéndolo en el código, ¡sólo será más difícil cambiarlo más tarde!

Nota: Para obtener más información sobre el modo estricto, consulte el Capítulo 5 del título Tipos y Gramática de esta serie.



