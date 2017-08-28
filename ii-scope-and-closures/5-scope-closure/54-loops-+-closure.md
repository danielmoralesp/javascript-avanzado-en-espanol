# 5.4 Loops + Closure

El ejemplo canónico más común usado para ilustrar el closure implica el humilde for-loop.

```js
for (var i=1; i<=5; i++) {
	setTimeout( function timer(){
		console.log( i );
	}, i*1000 );
}
```

**Nota**: Los linters a menudo se quejan al poner funciones dentro de bucles, porque los errores de no entender el closure son muy comunes entre los desarrolladores. Le explicamos cómo hacerlo correctamente aquí, aprovechando todo el poder de los closures. Pero esa sutileza se pierde a menudo en los linters y se quejarán de todos modos, asumiendo que usted no sabe realmente lo que usted está haciendo.

El espíritu de este fragmento de código es que normalmente esperamos que el comportamiento sea que los números "`1`", "`2`", .. "`5`" se imprimirían, uno a la vez, uno por segundo, respectivamente.

De hecho, si ejecuta este código, obtiene "`6`" impreso 5 veces, en los intervalos de un segundo.

**Huh?**

En primer lugar, vamos a explicar de dónde viene `6`. La condición de terminación del bucle es cuando `i` no es `<= 5`. La primera vez que es el caso es cuando `i` es `6`. Así pues, la salida está reflejando el valor final del `i` después de que el bucle termine.

Esto en realidad parece obvio a primera vista. Las devoluciones de llamada de la función `timer`  están funcionando bien después de completar el bucle. De hecho, a medida que pasan los temporizadores, aunque se estableciera `SetTimeout(..., 0)` en cada iteración, todas esas devoluciones de función seguirían funcionando estrictamente después de completar el bucle, e imprimir así 6 cada vez.

Pero hay una pregunta más profunda en juego aquí. ¿Qué falta en nuestro código para que se comporte como lo hemos hecho implícito semánticamente?

Lo que falta es que estamos tratando de implicar que cada iteración del bucle "captura" su propia copia de `i`, en el momento de la iteración. Pero, la forma en que funciona el ámbito, todas las 5 de esas funciones, aunque se definen por separado en cada iteración del bucle, todas están cerradas sobre el mismo ámbito global compartido, que tiene, de hecho, sólo un `i` en él.

Puesto de esa manera, por supuesto todas las funciones comparten una referencia a la misma `i`. Algo sobre la estructura del bucle tiende a confundirnos en pensar que hay algo más sofisticado en el trabajo. No hay. No hay diferencia que si cada uno de los 5 retornos de tiempo de espera se acaba de declarar uno después de la otra, sin bucle en absoluto.

OK, por lo tanto, de nuevo a nuestra pregunta caliente. ¿Qué falta? Necesitamos más alcance de closure. Específicamente, necesitamos un nuevo ámbito de closure para cada iteración del bucle.

En el Capítulo 3 aprendimos que el IIFE crea el ámbito declarando una función y ejecutándola inmediatamente.

Intentemos:

```js
for (var i=1; i<=5; i++) {
	(function(){
		setTimeout( function timer(){
			console.log( i );
		}, i*1000 );
	})();
}
```

¿Eso funciona? Intentalo. Una vez más, voy a esperar.

Terminaré el suspenso por ti. Nope. ¿Pero por qué? Ahora obviamente tenemos más alcance léxico. Cada devolución de llamada de la función de tiempo de espera se está cerrando realmente sobre su propio alcance de iteración creado respectivamente por cada IIFE.

No es suficiente tener un alcance para cerrar si ese ámbito está vacío. Mira de cerca. Nuestro IIFE es sólo un vacío, no hace nada. Necesita algo en él para que sea útil para nosotros.

Necesita su propia variable, con una copia del valor `i` en cada iteración.

```js
for (var i=1; i<=5; i++) {
	(function(){
		var j = i;
		setTimeout( function timer(){
			console.log( j );
		}, j*1000 );
	})();
}
```

**¡Eureka! ¡Funciona!**

Una pequeña variación que algunos prefieren es:

```js
for (var i=1; i<=5; i++) {
	(function(j){
		setTimeout( function timer(){
			console.log( j );
		}, j*1000 );
	})( i );
}
```

Por supuesto, puesto que estos IIFEs son sólo funciones, podemos pasar en `i`, y podemos llamarlo `j` si lo preferimos, o incluso podemos llamarlo `i` otra vez. De cualquier manera, el código funciona ahora.

> El uso de un IIFE dentro de cada iteración creó un nuevo alcance para cada iteración, lo que dio a nuestras devoluciones de llamada de función `timer`  la oportunidad de cerrar sobre un nuevo ámbito para cada iteración, una que tenía una variable con el valor de iteración adecuado para nosotros acceder.

¡Problema resuelto!

### Ámbito de bloque revisado

Mire atentamente nuestro análisis de la solución anterior. Utilizamos un IIFE para crear un nuevo ámbito por iteración. En otras palabras, en realidad necesitábamos un ámbito de bloque por iteración. El capítulo 3 nos mostró la declaración `let`, que secuestra un bloque y declara una variable allí mismo en el bloque.

**Esencialmente convierte un bloque en un ámbito que podemos cerrar**. Por lo tanto, el siguiente código impresionante "sólo funciona":

```js
for (var i=1; i<=5; i++) {
	let j = i; // yay, ámbito de bloque para cerrar (el closure)
	setTimeout( function timer(){
		console.log( j );
	}, j*1000 );
}
```

¡Pero eso no es todo! \(En mi mejor voz de Bob Barker\). Hay un comportamiento especial definido para las declaraciones utilizadas en la cabeza de un bucle `for`. Este comportamiento indica que la variable se declarará no sólo una vez para el bucle, sino para cada iteración. Y, por último, se inicializará en cada iteración posterior con el valor desde el final de la iteración anterior.

```js
for (let i=1; i<=5; i++) {
	setTimeout( function timer(){
		console.log( i );
	}, i*1000 );
}
```

¿Cuan genial es eso? El ámbito de bloque y el closure trabajando mano a mano, resolviendo todos los problemas del mundo. No sé para de ti, pero eso me convierte en un feliz JavaScripter.

