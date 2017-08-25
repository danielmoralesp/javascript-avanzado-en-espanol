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
    console.log( a );    // 10
})();

console.log( a );        // 42
```

Los `IIFE` también pueden tener valores de retorno:

```js
var x = (function IIFE(){
    return 42;
})();

x;    // 42
```

El valor `42` se devuelve de la función denominada `IIFE` que se ejecuta, y luego se asigna a `x`.

### Closure

El closure es uno de los conceptos más importantes, ya menudo menos comprendidos, en JavaScript. No lo voy a cubrir en detalle aquí y en su lugar le remito al título Scope & Closures de esta serie. Pero quiero decir algunas cosas sobre el para que entiendas el concepto general. Será una de las técnicas más importantes en su nivel de habilidades JS.

Puede pensar en el closure como una forma de "_**recordar**_" y seguir accediendo al ámbito de una función \(y sus variables\) incluso una vez que la función ha terminado de ejecutarse.

Considerar:

```js
function makeAdder(x) {
    // El parámetro `x` es una variable interna

    // función interna `add ()` usa `x`, así que
    // tiene un "closure" sobre él
    function add(y) {
        return y + x;
    };

    return add;
}
```

La referencia a la función interna `add(..)` que se retorna con cada llamada al `makeAdder(..)` externo es capaz de recordar cualquier valor `x` que se pasó en `makeAdder(..)`. Ahora, vamos a usar `makeAdder(..)`:

```js
// `plusOne` obtiene una referencia a la función `add(..)` interna
//  con closure sobre el parámetro `x` de
// el exterior de `makeAdder(..)`
var plusOne = makeAdder( 1 );

// `plusTen` obtiene una referencia a la función `add (..)`interna
// con closure sobre el parámetro `x` de
// el exterior `makeAdder (..)`
var plusTen = makeAdder( 10 );

plusOne( 3 );        // 4  <-- 1 + 3
plusOne( 41 );        // 42 <-- 1 + 41

plusTen( 13 );        // 23 <-- 10 + 13
```

Más información sobre cómo funciona este código:

1. Cuando llamamos `makeAdder(1)`, obtenemos una referencia a su `add(..)` interna que recuerda `x` como `1`. Llamamos a esta referencia de función `plusOne(..)`.
2. Cuando llamamos `makeAdder(10)`, obtenemos otra referencia a su `add(..)` interna que recuerda `x` como `10`. Llamamos a esta referencia de función `plusTen(..)`.
3. Cuando llamamos `plusOne(3)`, añade `3` \(su `y` interno\) al `1` \(recordado por `x`\), y obtenemos `4` como el resultado.
4. Cuando llamamos `plusTen(13)`, añade `13` \(su `y` interno\) al `10` \(recordado por `x`\), y obtenemos `23` como el resultado.

No se preocupe si esto parece extraño y confuso al principio - puede serlo! Tomará mucha práctica entenderlo completamente.

Pero confía en mí, una vez que lo hagas, es una de las técnicas más poderosas y útiles en toda la programación. Definitivamente vale la pena el esfuerzo para dejar que su cerebro cocine a fuego lento los closures poco a poco. En la siguiente sección, tendremos un poco más de práctica con los closures.

#### Modules

El uso más común de los closures en JavaScript es el patrón module. Los modules le permiten definir detalles de implementación privados \(variables, funciones\) ocultos del mundo exterior, así como una API pública accesible desde el exterior.

Considere:

```js
function User(){
	var username, password;

	function doLogin(user,pw) {
		username = user;
		password = pw;

		// do the rest of the login work
	}

	var publicAPI = {
		login: doLogin
	};

	return publicAPI;
}

// create a `User` module instance
var fred = User();

fred.login( "fred", "12Battery34!" );
```

La función `User()` sirve como un ámbito externo que contiene las variables `username` y `password`, así como la función interna `doLogin()`; Estos son todos los detalles internos privados de este módulo de usuario que no se puede acceder desde el mundo exterior.

**Advertencia**: No estamos llamando a `new User()` aquí, a propósito, a pesar del hecho de que probablemente parece más común a la mayoría de los lectores. `User()` es sólo una función, no una clase a instanciar, por lo que se llama normalmente. El uso de `new` sería inapropiado y realmente desperdiciaría recursos.

Ejecutar `User()` crea una instancia del módulo `User` - se crea un nuevo ámbito y, por lo tanto, una copia completamente nueva de cada una de estas variables/funciones internas. Asignamos esta instancia a `fred`. Si ejecutamos `User()` de nuevo, obtendremos una nueva instancia completamente distinta de `fred`.

La función interna `doLogin()` tiene un closure sobre el `username` y `password `, lo que significa que conservará su acceso a ellos incluso después de que la función `User()` termine de ejecutarse.

`PublicAPI` es un objeto con una propiedad/método en él, `login`, que es una referencia a la función interna `doLogin()`. Cuando devolvemos `publicAPI` de `User()`, se convierte en la instancia que llamamos `fred`.

En este punto, la función externa `User()` ha terminado de ejecutarse. Normalmente, se piensa que las variables internas como `username` y `password` se han ido. Pero aquí no lo han hecho, porque hay un closure en la función `login()` que los mantiene vivos.

Es por eso que podemos llamar a `fred.login(..)` - lo mismo que llamar a la interna `doLogin(..)` - y todavía puede acceder a las variables internas de `username` y `password`.

Hay una buena probabilidad de que con sólo este breve vistazo al closures y el patrón module, algunos de ellos sean todavía un poco confusos. ¡Está bien! Toma un poco de trabajo para envolver su cerebro alrededor de él.

A partir de aquí, vaya a leer el título Scope & Closures de esta serie para una exploración mucho más profunda.

