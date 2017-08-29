# 1.2 Confusiones

Pronto comenzaremos a explicar cómo `this`  funciona realmente, pero primero debemos disipar algunos conceptos erróneos sobre cómo realmente NO funciona.

El nombre "this" crea confusión cuando los desarrolladores tratan de pensarlo demasiado literalmente. Hay dos significados a menudo asumidos, pero ambos son incorrectos.

### Itself

La primera tentación común es asumir que `this` se refiere a la función misma. Esa es una inferencia gramatical razonable, por lo menos.

¿Por qué quieres referirte a una función desde dentro de sí mismo? Las razones más comunes serían cosas como la recursión \(llamando a una función desde dentro de sí misma\) o tener un manejador de eventos que pueda desvincularse cuando se llama por primera vez.

Los desarrolladores nuevos en los mecanismos de JS a menudo piensan que hacer referencia a la función como un objeto \(todas las funciones en JavaScript son objetos!\) le permite almacenar el estado \(valores en las propiedades\) entre las llamadas de función. Aunque esto es ciertamente posible y tiene algunos usos limitados, el resto del libro explicará en muchos otros patrones para mejores lugares para almacenar el estado además del objeto de función.

Pero por un momento exploraremos ese patrón, para ilustrar cómo `this` no permite que una función obtenga una referencia a sí misma tal como podríamos haber asumido.

Considere el siguiente código, donde intentamos rastrear cuántas veces se llamó una función \(`foo`\):

```js
function foo(num) {
    console.log( "foo: " + num );

    // keep track of how many times `foo` is called
    this.count++;
}

foo.count = 0;

var i;

for (i=0; i<10; i++) {
    if (i > 5) {
        foo( i );
    }
}
// foo: 6
// foo: 7
// foo: 8
// foo: 9

// how many times was `foo` called?
console.log( foo.count ); // 0 -- WTF?
```

`foo.count` sigue siendo `0`, aunque las cuatro sentencias `console.log` indican claramente que `foo(...)` se llamó en realidad cuatro veces. La frustración se deriva de una interpretación demasiado literal de lo que significa \(en `this.count++`\).

Cuando el código ejecuta `foo.count = 0`, de hecho agrega una propiedad `count` al objeto de función `foo`. Sin embargo, para la referencia `this.count` dentro de la función, `this` no apunta en absoluto a ese objeto de función, y así, aunque los nombres de propiedad son los mismos, los objetos raíz son diferentes y se produce confusión.

**Nota**: Un desarrollador responsable debe preguntar en este momento: "Si estaba incrementando una propiedad `count` pero no era la que esperaba, ¿cuál fue el `count` que incrementé?" De hecho, si cavara más profundamente, descubriría que había creado accidentalmente una variable global `count` \(véase el Capítulo 2 para saber cómo sucedió!\), Y actualmente tiene el valor `NaN`. Por supuesto, una vez que identifica este resultado peculiar, entonces tiene un conjunto de preguntas más: "¿Cómo era global, y por qué terminó `NaN` en lugar de algún valor `count` adecuado?" \(Véase el capítulo 2\).

En lugar de detenerse en este punto y cavar en por qué la referencia `this` no parece estar comportándose como se esperaba, y responder a esas preguntas difíciles pero importantes, muchos desarrolladores simplemente evitan el problema en conjunto, y hackean hacia otra solución, como la creación de otro Objeto para contener la propiedad `count`:

```js
function foo(num) {
    console.log( "foo: " + num );

    // keep track of how many times `foo` is called
    data.count++;
}

var data = {
    count: 0
};

var i;

for (i=0; i<10; i++) {
    if (i > 5) {
        foo( i );
    }
}
// foo: 6
// foo: 7
// foo: 8
// foo: 9

// how many times was `foo` called?
console.log( data.count ); // 4
```

Si bien es cierto que este enfoque "resuelve" el problema, desafortunadamente simplemente ignora el problema real -la falta de comprensión de lo que `this` significa y cómo funciona- y vuelve a caer en la zona de confort de un mecanismo más familiar: alcance léxico .

**Nota**: El alcance léxico es un mecanismo perfectamente fino y útil; No estoy menospreciando el uso de la misma, por cualquier medio \(ver el título "Scope & Closures" de esta serie de libros\). Pero constantemente adivinar cómo usar `this`, y por lo general estar equivocado, no es una buena razón para retroceder de nuevo al ámbito léxico y nunca aprender por qué esto le escapa.

Para hacer referencia a un objeto de función desde dentro de sí mismo, `this` por sí mismo suele ser insuficiente. Generalmente se necesita una referencia al objeto de función a través de un identificador léxico \(variable\) que apunte hacia él.

Considere estas dos funciones:

```js
function foo() {
    foo.count = 4; // `foo` refers to itself
}

setTimeout( function(){
    // anonymous function (no name), cannot
    // refer to itself
}, 10 );
```

En la primera función, llamada "función nombrada", `foo` es una referencia que se puede utilizar para referirse a la función desde dentro de sí misma.

Pero en el segundo ejemplo, la función de devolución de llamada pasada a `setTimeout(..)` no tiene ningún identificador de nombre \(llamada "función anónima"\), por lo que no hay forma adecuada de referirse al objeto de función en sí misma.

**Nota**: La referencia a la vieja escuela, pero ahora desaprobada y con el ceño fruncido `arguments.callee`. La referencia dentro de una función también apunta al objeto de función de la función en ejecución. Esta referencia es típicamente la única manera de acceder al objeto de una función anónima desde dentro de sí misma. El mejor enfoque, sin embargo, es evitar el uso de funciones anónimas por completo, al menos para aquellos que requieren una auto-referencia, y en su lugar utilizar una función nombrada \(expresión\). `arguments.callee` está obsoleto y no debe utilizarse.

Por lo tanto, otra solución a nuestro ejemplo de ejecución habría sido usar el identificador `foo` como una referencia de objeto de función en cada lugar, y no usar `this` en absoluto, lo cual funciona:

```js
function foo(num) {
	console.log( "foo: " + num );

	// keep track of how many times `foo` is called
	foo.count++;
}

foo.count = 0;

var i;

for (i=0; i<10; i++) {
	if (i > 5) {
		foo( i );
	}
}
// foo: 6
// foo: 7
// foo: 8
// foo: 9

// how many times was `foo` called?
console.log( foo.count ); // 4
```

Sin embargo, ese enfoque de manera similar a los pasos laterales de la comprensión real de `this` y se basa enteramente en el ámbito léxico de la variable `foo`.

Sin embargo, otra forma de abordar el problema es forzar `this` a apuntar realmente al objeto de función `foo`:

```js
function foo(num) {
	console.log( "foo: " + num );

	// keep track of how many times `foo` is called
	// Note: `this` IS actually `foo` now, based on
	// how `foo` is called (see below)
	this.count++;
}

foo.count = 0;

var i;

for (i=0; i<10; i++) {
	if (i > 5) {
		// using `call(..)`, we ensure the `this`
		// points at the function object (`foo`) itself
		foo.call( foo, i );
	}
}
// foo: 6
// foo: 7
// foo: 8
// foo: 9

// how many times was `foo` called?
console.log( foo.count ); // 4
```

**En lugar de evitar `this`, lo abrazamos**. Vamos a explicar en un momento cómo funcionan estas técnicas de trabajo mucho más completamente, por lo que no te preocupes si todavía estás un poco confundido!

### Su alcance

El siguiente error más común sobre el significado de `this` es que de alguna manera se refiere al alcance de la función. Es una pregunta difícil, porque en cierto sentido hay algo de verdad, pero en el otro sentido, es bastante equivocado.

Para ser claro, `this` no se refiere, en modo alguno, al **ámbito léxico** de una función. Es cierto que internamente, el alcance es como un objeto con propiedades para cada uno de los identificadores disponibles. Pero el "objeto" de alcance no es accesible al código JavaScript. Es una parte interna de la implementación del Motor.

Considere el código que intenta \(y falla!\) cruzar el límite y usa `this` para referirse implícitamente al ámbito léxico de una función:

```js
function foo() {
	var a = 2;
	this.bar();
}

function bar() {
	console.log( this.a );
}

foo(); //undefined
```

Hay más de un error en este fragmento. Aunque puede parecer artificial, el código que ves es una destilación del código real del mundo real que se ha intercambiado en los foros públicos de ayuda de la comunidad. Es una ilustración maravillosa \(si no triste\) de lo equivocados que pueden ser estos supuestos sobre `this`.

En primer lugar, se intenta hacer referencia a la función `bar()` a través de `this.bar()`. Es casi ciertamente un accidente que funcione, pero vamos a explicar el cómo en breve. La forma más natural de haber invocado `bar()` habría sido omitir la declaración `this`. Y sólo hacer una referencia léxica al identificador.

Sin embargo, el desarrollador que escribe este código intenta usar `this` para crear un puente entre los ámbitos léxicos de `foo()` y `bar()`, de modo que `bar()` tenga acceso a la variable `a` en el ámbito interno de `foo()`. **Ningún puente es posible**. No puedes usar esta referencia para buscar algo en un ámbito léxico. No es posible.

Cada vez que se sientan tratando de mezclar perspectivas de alcance léxico con `this`, recuerde: no hay puente.

