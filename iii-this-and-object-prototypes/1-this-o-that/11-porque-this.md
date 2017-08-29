# 1.1 ¿Porque `this`?

Si el mecanismo `this` es tan confuso, incluso para los desarrolladores de JavaScript experimentados, uno puede preguntarse por qué es incluso útil? ¿Es más problemático de lo que vale? Antes de saltar en el cómo, debemos examinar el por qué.

Intentemos ilustrar la motivación y la utilidad de `this`:

```js
function identify() {
	return this.name.toUpperCase();
}

function speak() {
	var greeting = "Hello, I'm " + identify.call( this );
	console.log( greeting );
}

var me = {
	name: "Kyle"
};

var you = {
	name: "Reader"
};

identify.call( me ); // KYLE
identify.call( you ); // READER

speak.call( me ); // Hello, I'm KYLE
speak.call( you ); // Hello, I'm READER
```

Si el cómo de este fragmento te confunde, ¡no te preocupes! Llegaremos a eso en breve. Simplemente deja esas preguntas de lado por ahora para que podamos estudiar el porqué más claramente.

Este fragmento de código permite que las funciones `identifier()` y `speak()` sean reutilizadas en contextos múltiples \(`me` y `you`\), en lugar de necesitar una versión separada de la función para cada objeto.

En lugar de confiar en `this`, usted podría haberlo pasado explícitamente en un objeto de contexto tanto para `identifier()` como para `speak()`.

```js
function identify(context) {
	return context.name.toUpperCase();
}

function speak(context) {
	var greeting = "Hello, I'm " + identify( context );
	console.log( greeting );
}

var me = {
	name: "Kyle"
};

var you = {
	name: "Reader"
};

identify( you ); // READER
speak( me ); // Hello, I'm KYLE
```

Sin embargo, el mecanismo `this` proporciona una forma más elegante de "pasar" implícitamente una referencia de objeto, lo que conduce a un diseño de API más limpio y una reutilización más fácil.

Cuanto más complejo es su patrón de uso, más claramente verá que pasar el contexto alrededor como un parámetro explícito es a menudo más desordenado que pasar alrededor del contexto de `this`. Cuando exploramos objetos y prototipos, veremos la utilidad de una colección de funciones que pueden referenciar automáticamente al objeto de contexto apropiado.







