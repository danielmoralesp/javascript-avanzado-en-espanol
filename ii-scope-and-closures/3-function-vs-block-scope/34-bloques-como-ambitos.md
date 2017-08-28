# 3.4 Bloques como ámbitos

Si bien las funciones son la unidad de ámbito más común, y ciertamente la más amplia de los enfoques de diseño en la mayoría de los códigos JS en circulación, otras unidades de alcance son posibles, y el uso de estas otras unidades de alcance puede conducir aún mejor, más limpio y código mantenible.

Muchos lenguajes distintos de JavaScript soportan Block Scope, por lo que los desarrolladores de esos lenguajes están acostumbrados a esa mentalidad, mientras que aquellos que sólo han trabajado principalmente en JavaScript pueden encontrar el concepto un poco extraño.

Pero incluso si usted nunca ha escrito una sola línea de código en forma de bloque de ámbito, usted todavía está probablemente familiarizado con este lenguaje muy común en JavaScript:

```js
for (var i=0; i<10; i++) {
	console.log( i );
}
```

Declaramos la variable `i` directamente dentro de la cabeza del bucle `for`, lo más probable es que nuestra intención sea usar `i` sólo dentro del contexto de ese bucle `for`, y esencialmente ignorar el hecho de que la variable realmente se alcanza al ámbito de inclusión \(función o global\).

Eso es a lo que se refiere el alcance de bloque. Declarar las variables lo más cerca posible, lo más local posible, a donde se van a utilizar. Otro ejemplo:

```js
var foo = true;

if (foo) {
	var bar = foo * 2;
	bar = something( bar );
	console.log( bar );
}
```

Estamos usando una variable `bar` sólo en el contexto de la sentencia `if`, por lo que hace sentido que lo declaremos dentro del `if-block`. Sin embargo, cuando declaramos variables no es relevante cuando se utiliza `var`, porque siempre pertenecerán al ámbito de inclusión. Este fragmento es esencialmente "falso" de bloque de alcance, por razones estilísticas, y confiar en la auto-ejecución no accidentalmente utilizar `bar` en otro lugar en ese ámbito.

El alcance del bloque es una herramienta para extender el "Principio de Exposición al Menor Privilegio" anterior desde ocultar información en funciones a ocultar información en bloques de nuestro código.

Considere nuevamente el ejemplo for-loop:

```js
for (var i=0; i<10; i++) {
	console.log( i );
}
```

¿Por qué contaminar todo el ámbito de una función con la variable `i` que sólo va a ser \(o sólo debería ser, al menos\) utilizada para el bucle `for`?

Pero lo que es más importante, los desarrolladores pueden preferir revisarse contra el uso \(re\) accidental de variables ajenas a su propósito, como ser emitido un error sobre una variable desconocida si intenta utilizarlo en el lugar equivocado. Bloquear el ámbito \(si fuera posible\) para la variable `i` haría `i` disponible sólo para el bucle `for`, causando un error si se accede a otro lugar en la función. Esto ayuda a asegurar que las variables no se vuelvan a utilizar en formas confusas o difíciles de mantener.

Pero, la triste realidad es que, en la superficie, JavaScript no tiene ninguna facilidad para el alcance del bloque.

Es decir, hasta que hagas un poco más.

### `with`

Hemos aprendido acerca de `with` en el Capítulo 2. Si bien es un fruncido a construir, es un ejemplo de \(una forma de\) ámbito de bloque, en el que el ámbito que se crea desde el objeto sólo existe para la vida de `with` con la declaración, y no en el ámbito circundante.

### `try/catch`

Es un hecho muy poco conocido que JavaScript en ES3 especificó la declaración de variable en la cláusula `catch` de un `try/catch` para ser bloqueado al bloque `catch`.

Por ejemplo:

```js
try {
	undefined(); // illegal operation to force an exception!
}
catch (err) {
	console.log( err ); // works!
}

console.log( err ); // ReferenceError: `err` not found
```

Como puede ver, `err` existe sólo en la cláusula `catch` y lanza un error cuando intenta hacer referencia a él en otro lugar.

**Nota**: Si bien este comportamiento se ha especificado y es cierto de prácticamente todos los entornos JS estándar \(excepto quizás en un IE antiguo\), muchos linters parecen todavía quejarse si tiene dos o más cláusulas de `catch` en el mismo ámbito que cada declaración de su variable de error con el mismo nombre del identificador. Esto no es en realidad una nueva definición, ya que las variables son de bloque con seguridad de alcance, pero los linters todavía parecen molestarse, y se quejan de este hecho.

Para evitar estas advertencias innecesarias, algunos devs nombrarán sus variables de `catch` `err1`, `err2`, etc. Otros devs simplemente desactivarán la comprobación de linting para nombres de variables duplicados.

La naturaleza de alcance de `catch` puede parecer un hecho académico inútil, pero vea el Apéndice B para más información sobre lo útil que podría ser.

### `let`

Hasta ahora, hemos visto que JavaScript sólo tiene algunos comportamientos de nicho extraño que exponen la funcionalidad de ámbito de bloque. Si eso fuera todo lo que tenemos, y de hecho tuvimos por muchos, muchos años, entonces el alcance de bloque no sería útil para el desarrollador de JavaScript.

Afortunadamente, ES6 cambia eso, e introduce una nueva palabra clave `let` que se pone junto a `var` como otra forma de declarar variables.

La palabra clave `let` agrega la declaración de la variable al ámbito de cualquier bloque \(comúnmente un par `{..} `\) en el que está contenido. En otras palabras, deje implícitamente el alcance de cualquier bloque para su declaración de variable.

```js
var foo = true;

if (foo) {
	let bar = foo * 2;
	bar = something( bar );
	console.log( bar );
}

console.log( bar ); // ReferenceError
```

El uso de `let` para adjuntar una variable a un bloque existente es algo implícito. Puede confundirte si no estás prestando mucha atención a los bloques que tienen variables de alcance para ellos, y tienen el hábito de mover bloques alrededor, envolverlos en otros bloques, etc, a medida que desarrolla y evoluciona código.

La creación de bloques explícitos para el alcance de bloque puede solucionar algunas de estas preocupaciones, haciéndolo más evidente de dónde se adjuntan las variables y dónde no. Normalmente, el código explícito es preferible sobre código implícito o sutil. Este estilo de ámbito de bloque explícito es fácil de lograr y se adapta más naturalmente a cómo funciona el alcance de bloque en otros idiomas:

```js
var foo = true;

if (foo) {
	{ // <-- explicit block
		let bar = foo * 2;
		bar = something( bar );
		console.log( bar );
	}
}

console.log( bar ); // ReferenceError
```

Podemos crear un bloque arbitrario al que debemos enlazar simplemente incluyendo un par `{..}` dondequiera que una sentencia sea gramática válida. En este caso, hemos hecho un bloque explícito dentro de la instrucción `if`, que puede ser más fácil como un bloque entero para moverse más adelante en la refactorización, sin afectar la posición y la semántica de la instrucción `if` que encierra.

**Nota**: Para otra forma de expresar los ámbitos explícitos de bloque, consulte el Apéndice B.

En el capítulo 4, abordaremos el hoisting, que habla de que las declaraciones se toman como existentes para todo el ámbito en el que se producen.

Sin embargo, las declaraciones hechas con `let` no se "hoistean" a todo el alcance del bloque en el que aparecen. Tales declaraciones no "existirán" observables en el bloque hasta la declaración.

```js
{
   console.log( bar ); // ReferenceError!
   let bar = 2;
}
```

#### Garbage Collection

Otra razón de porque bloquear el alcance es útil se refiere a los closures y la Garbage Collection \(recolección de basura\) para recuperar la memoria. Lo vamos a ilustrar brevemente aquí, pero el mecanismo de closure se explica en detalle en el capítulo 5.

Considere:

```js
function process(data) {
	// do something interesting
}

var someReallyBigData = { .. };

process( someReallyBigData );

var btn = document.getElementById( "my_button" );

btn.addEventListener( "click", function click(evt){
	console.log("button clicked");
}, /*capturingPhase=*/false );
```

La función `click`  de devolución de llamada no necesita la variable `someReallyBigData` en absoluto. Eso significa, teóricamente, después de que el proceso `(..) `se ejecute, la estructura de datos de memoria grande podría ser Garbage Collection. Sin embargo, es bastante probable \(aunque depende de la implementación\) que el motor JS todavía tenga que mantener la estructura alrededor, ya que la función `click` tiene un closure en todo el ámbito.

Bloquear el alcance puede abordar esta preocupación, por lo que es más claro para el motor que no es necesario mantener `someReallyBigData` alrededor:

```js
function process(data) {
	// do something interesting
}

// anything declared inside this block can go away after!
{
	let someReallyBigData = { .. };

	process( someReallyBigData );
}

var btn = document.getElementById( "my_button" );

btn.addEventListener( "click", function click(evt){
	console.log("button clicked");
}, /*capturingPhase=*/false );
```

Declarar bloques explícitos para que las variables se enlacen localmente es una poderosa herramienta que puede agregar a su caja de herramientas de código.

#### Bucles `let`

Un caso particular donde `let` brilla está en el caso del ciclo `for` como ha sido discutido previamente.

```js
for (let i=0; i<10; i++) {
	console.log( i );
}

console.log( i ); // ReferenceError
```

No sólo deja en el encabezado del for-loop unido el `i` al cuerpo del for-loop, sino que de hecho, vuelve a enlazarlo a cada iteración del loop, asegurándose de volver a asignar el valor desde el final de la iteración del bucle anterior.

He aquí otra manera de ilustrar el comportamiento de vinculación por iteración que se produce:

```js
{
	let j;
	for (j=0; j<10; j++) {
		let i = j; // re-bound for each iteration!
		console.log( i );
	}
}
```

La razón por la cual esta vinculación por iteración es interesante será aclarada en el Capítulo 5 cuando discutamos closures.

Debido a que las declaraciones se unen a bloques arbitrarios en lugar de al ámbito de la función adjunta \(o global\), puede haber esquemas en los que el código existente tiene una dependencia oculta en las declaraciones de `var` de la función, y reemplazar la `var` por `let` puede requerir atención adicional al refactorizar el código.

Considere:

```js
var foo = true, baz = 10;

if (foo) {
	var bar = 3;

	if (baz > bar) {
		console.log( baz );
	}

	// ...
}
```

Este código es fácil re-factorizarlo como:

```js
var foo = true, baz = 10;

if (foo) {
	var bar = 3;

	// ...
}

if (baz > bar) {
	console.log( baz );
}
```

Pero, tenga cuidado con estos cambios cuando se usan variables de alcance de bloque:

```js
var foo = true, baz = 10;

if (foo) {
	let bar = 3;

	if (baz > bar) { // <-- don't forget `bar` when moving!
		console.log( baz );
	}
}
```

Vea el Apéndice B para un estilo alterno \(más explícito\) de alcance de bloque que puede proporcionar un código más fácil de mantener/refactorizar que es más robusto a estos escenarios.

### `const`

Además de `let`, ES6 introduce `const`, que también crea una variable de ámbito de bloque, pero cuyo valor es fijo \(constante\). Cualquier intento de cambiar ese valor en un momento posterior da como resultado un error.

```js
var foo = true;

if (foo) {
	var a = 2;
	const b = 3; // block-scoped to the containing `if`

	a = 3; // just fine!
	b = 4; // error!
}

console.log( a ); // 3
console.log( b ); // ReferenceError!
```



