# 4.2 El compilador pega de nuevo

Para responder a esta pregunta, necesitamos referirnos al Capítulo 1, y nuestra discusión sobre los compiladores. Recuerde que el motor realmente compilará su código JavaScript antes de que lo interprete. Parte de la fase de compilación fue encontrar y asociar todas las declaraciones con sus alcances apropiados. El capítulo 2 nos mostró que este es el corazón del alcance léxico.

Por lo tanto, la mejor manera de pensar acerca de las cosas es que todas las declaraciones, tanto las variables como las funciones, se procesan primero, antes de ejecutar cualquier parte de su código.

Cuando ve` var a = 2 ;`, probablemente piensa en eso como una sentencia. Pero JavaScript realmente piensa en ello como dos declaraciones: `var a;` Y `a = 2 ;`. La primera declaración, la declaración, se procesa durante la fase de compilación. La segunda sentencia, la asignación, se deja en su lugar para la fase de ejecución.

Nuestro primer fragmento entonces debe ser pensado como si estuviera siendo manejado igual a esto:

```js
var a;
```

```js
a = 2;

console.log( a );
```

... donde la primera parte es la compilación y la segunda parte es la ejecución.

Del mismo modo, nuestro segundo fragmento se procesa realmente como:

```js
var a;
```

```js
console.log( a );

a = 2;
```

Por lo tanto, una forma de pensar, de manera metafórica, sobre este proceso, es que las declaraciones de variables y de funciones se "mueven" desde donde aparecen en el flujo del código hasta la parte superior del código. Esto da lugar al nombre "Hoisting".

En otras palabras, **el huevo \(declaración\) viene antes de la gallina \(asignación\).**

**Nota**: Solamente las declaraciones son subidas \(hoisted\), mientras que cualquier asignación u otra lógica ejecutable se deja en su lugar. Si la subida fuera a reorganizar la lógica ejecutable de nuestro código, eso podría causar estragos.

```js
foo();

function foo() {
	console.log( a ); // undefined

	var a = 2;
}
```

Se sube la declaración de la función `foo` \(que en este caso incluye el valor implícito de ella como una función real\), de modo que la llamada en la primera línea pueda ejecutarse.

También es importante tener en cuenta que la subida es por alcance. Así, mientras que nuestros fragmentos anteriores se simplificaron en que sólo incluían alcance global, la función `foo(..)` que ahora estamos examinando muestra que `var a` se sube a la parte superior de `foo(..)` \(no, obviamente, a la parte superior del programa\). Por lo tanto, el programa puede interpretarse de la siguiente manera:

```js
function foo() {
	var a;

	console.log( a ); // undefined

	a = 2;
}

foo();
```

Las declaraciones de funciones se suben, como acabamos de ver. Pero las expresiones funcionales no se suben.

```js
foo(); // not ReferenceError, but TypeError!

var foo = function bar() {
	// ...
};
```

El identificador de variable `foo` se sube y se adjunta al ámbito de inclusión \(global\) de este programa, por lo que f`oo()` no falla como `ReferenceError`. Pero `foo` no tiene ningún valor todavía \(como si hubiera sido una declaración de función verdadera en lugar de expresión\). Por tanto, `foo()` intenta invocar el valor indefinido, que es una operación `TypeError` ilegal.

También recuerde que aunque es una expresión de función nombrada, el identificador de nombre no está disponible en el ámbito de inclusión:

```js
foo(); // TypeError
bar(); // ReferenceError

var foo = function bar() {
	// ...
};
```

Este fragmento se interpreta con mayor precisión \(con hoisting\) como:

```js
var foo;

foo(); // TypeError
bar(); // ReferenceError

foo = function() {
	var bar = ...self...
	// ...
}
```



