# 2.5 Funciones como Valores

Hasta ahora, hemos discutido las funciones como el principal mecanismo de scope en JavaScript. Puede recordar la sintaxis típica de la declaración de funciones de la siguiente manera:

```js
function foo() {
    // ..
}
```

Aunque no parezca obvio de esa sintaxis, `foo` es básicamente sólo una variable en el ámbito exterior que incluye una referencia a la función que se está declarando. Es decir, la función en sí es un valor, al igual que sería `42` o `[1,2,3]` .

Esto puede sonar como un concepto extraño al principio, así que tómese un momento para reflexionar sobre él. No sólo puede pasar un valor \(argumento\) a una función, sino que una función en sí puede ser un valor que se asigna a variables, o se pasa a, o se devuelve a otras funciones.

Como tal, un valor de función debe ser pensado como una expresión, al igual que cualquier otro valor o expresión.

Considerar:

```js
var foo = function() {
    // ..
};

var x = function bar(){
    // ..
};
```

La primera expresión de función asignada a la variable `foo` se llama anónima porque no tiene nombre.

La segunda expresión de función llamada \(`bar`\), incluso como una referencia a ella también se asigna a la variable `x`. Las expresiones de función nombradas son generalmente más preferibles, aunque las expresiones de función anónimas todavía son extremadamente comunes.

Para obtener más información, consulte el título Scope & Closures de esta serie.

### Immediately Invoked Function Expressions \(IIFEs\) -  Expresiones de función invocadas Inmediatamente

En el snippet anterior, ninguna de las expresiones de función se ejecutan - podríamos si hubiéramos incluido `foo()` o `x()`, por ejemplo.

Hay otra forma de ejecutar una expresión de función, que normalmente se denomina expresión de función inmediatamente invocada \(IIFE\):

```js
(function IIFE(){
    console.log( "Hello!" );
})();
// "Hello!"
```

El exterior `(..)` que rodea la expresión de función `(función IIFE () {..})` es sólo un matiz de la gramática JS necesaria para evitar que se trate como una declaración de función normal.

El final `()` al final de la expresión - el `}) ()`;  - es lo que en realidad ejecuta la expresión de función referenciada inmediatamente antes de ella.

Eso puede parecer extraño, pero no es tan extraño como a primera vista. Considere las similitudes entre `foo` y `IIFE` aquí:

```js
function foo() { .. }

// `foo` function reference expression,
// then `()` executes it
foo();

// expresión de función `IIFE`,
// entonces `()` lo ejecuta
(function IIFE(){ .. })();
```

Como se puede ver, la lista de la función `(IIFE () {..})` antes de su ejecución `()` es esencialmente el mismo que incluye `foo` antes de su ejecución `()`; En ambos casos, la referencia de función se ejecuta con `()` inmediatamente después de ella.

Debido a que un `IIFE` es sólo una función y las funciones crean un ámbito variable, el uso de un `IIFE` de esta manera se utiliza a menudo para declarar variables que no afectarán al código circundante fuera del `IIFE`:

```js
var a = 42;

(function IIFE(){
	var a = 10;
	console.log( a );	// 10
})();

console.log( a );		// 42
```

Los `IIFE` también pueden tener valores de retorno:

```js
var x = (function IIFE(){
	return 42;
})();

x;	// 42
```

El valor `42` se devuelve de la función denominada `IIFE` que se ejecuta, y luego se asigna a `x`.

