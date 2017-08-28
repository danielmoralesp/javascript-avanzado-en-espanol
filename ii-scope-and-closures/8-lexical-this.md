# 8- Lexical-this

Aunque este título no aborda este mecanismo `this` en detalle, hay un tema ES6 que relaciona esto con el alcance léxico de una manera importante, que examinaremos rápidamente.

ES6 agrega una forma sintáctica especial de declaración de función llamada "función de flecha \(arrow function\)". Se parece a esto:

```js
var foo = a => {
	console.log( a );
};

foo( 2 ); // 2
```

La llamada "flecha gorda \(fat arrow\)" se menciona a menudo como una palabra corta para la palabra clave de `function` tediosamente verbosa \(sarcasmo\).

Pero hay algo mucho más importante en las funciones de flecha que no tiene nada que ver con el ahorro de pulsaciones de teclado en su declaración.

En pocas palabras, este código sufre un problema:

```js
var obj = {
	id: "awesome",
	cool: function coolFn() {
		console.log( this.id );
	}
};

var id = "not awesome";

obj.cool(); // awesome

setTimeout( obj.cool, 100 ); // not awesome
```

El problema es la pérdida de `this` binding en la función `cool()`. Hay varias maneras de abordar ese problema, pero una solución frecuentemente repetida es `var self = this ;`.

Que podría parecer:

```js
var obj = {
	count: 0,
	cool: function coolFn() {
		var self = this;

		if (self.count < 1) {
			setTimeout( function timer(){
				self.count++;
				console.log( "awesome?" );
			}, 100 );
		}
	}
};

obj.cool(); // awesome?
```

Sin llegar demasiado a las malas hierbas aquí, la "solución" `var self = this`  simplemente dispensa todo el problema de la comprensión y el uso adecuado de `this` binding, y en su lugar vuelve a algo con lo que estamos quizás más cómodos: ámbito léxico. El `self` se convierte en un simple identificador que puede ser resuelto a través del alcance léxico y el closure, y no se preocupa de lo que le pasó a el binding de `this` a lo largo del camino.

A la gente no le gusta escribir cosas verbosas, especialmente cuando lo hacen una y otra vez. Por lo tanto, una motivación de ES6 es ayudar a aliviar estos escenarios, y, de hecho, corregir problemas del lenguaje común, como este.

La solución ES6, la función de flecha, introduce un comportamiento llamado "lexical this".

```js
var obj = {
	count: 0,
	cool: function coolFn() {
		if (this.count < 1) {
			setTimeout( () => { // arrow-function ftw?
				this.count++;
				console.log( "awesome?" );
			}, 100 );
		}
	}
};

obj.cool(); // awesome?
```

La breve explicación es que las funciones de flecha no se comportan en absoluto como las funciones normales cuando se trata de la vinculación de `this`. Descartan todas las reglas normales para la vinculación de `this`, y en cambio asumen el valor de `this` y el alcance inmediato de su inclusión léxica, cualquiera que sea.

Por lo tanto, en ese fragmento, la función de flecha no obtiene su `this` sin consolidarlo de alguna manera impredecible, simplemente "hereda" el `this` de la función `cool()` \(que es correcto si lo invocamos como se muestra!\).

Mientras que esto hace para el código más corto, mi perspectiva es que las funciones de la flecha son realmente apenas las que codifican en la sintaxis del lenguaje un error común de los reveladores, que es confundir y combinar reglas "que vinculan" con las reglas de "alcance lexical".

Dicho de otra manera: ¿por qué ir al problema y la verbosidad de usar el paradigma del estilo `this` de codificación, sólo para cortar en las rodillas, mezclándolo con referencias léxicas. Parece natural abrazar un enfoque o el otro para cualquier pieza de código, y no mezclarlos en el mismo código.

**Nota**: otra detracción de las funciones de flecha es que son anónimas, no nombradas. Consulte el Capítulo 3 para ver las razones por las cuales las funciones anónimas son menos deseables que las funciones con nombre.

Un enfoque más apropiado, en mi perspectiva, a este "problema", es usar y abrazar correctamente el mecanismo `this`.

```js
var obj = {
	count: 0,
	cool: function coolFn() {
		if (this.count < 1) {
			setTimeout( function timer(){
				this.count++; // `this` is safe because of `bind(..)`
				console.log( "more awesome" );
			}.bind( this ), 100 ); // look, `bind()`!
		}
	}
};

obj.cool(); // more awesome
```

Ya sea que prefiera el nuevo vocabulario léxico-this comportamiento de las funciones de las flechas, o prefiere el `bind()` probado-y-`true()`, es importante tener en cuenta que las funciones flecha no son solo para escribir abreviadamente "function".

Tienen una diferencia de comportamiento intencional que debemos aprender y entender, y si así lo deseamos, aprovechar.

Ahora que comprendemos plenamente el alcance léxico \(y el closure!\), La comprensión léxico-this debe ser una brisa!

