# 2.2 Nada más que reglas

Volvamos nuestra atención ahora a cómo el sitio de llamada determina dónde apuntará a `this` durante la ejecución de una función.

Debe inspeccionar el sitio de llamada y determinar cuál de las 4 reglas se aplica. Primero explicaremos cada una de estas 4 reglas de forma independiente, y luego ilustraremos su orden de precedencia, si se pudieran aplicar múltiples reglas al sitio de llamada.

### Vinculación por defecto

La primera regla que examinaremos viene del caso más común de las llamadas de función: invocación de función autónoma. Piense en esta regla `this` como la regla de captura por defecto cuando no se aplica ninguna otra regla.

Considere este código:

```js
function foo() {
    console.log( this.a );
}

var a = 2;

foo(); // 2
```

Lo primero que hay que tener en cuenta es que si las variables declaradas en el ámbito global, como `var a = 2` , son sinónimas con propiedades de objeto global del mismo nombre. No son copias el uno del otro, son el uno al otro. Piense en ello como dos caras de la misma moneda.

En segundo lugar, vemos que cuando `foo()` se llama, `this.a` se resuelve a nuestra variable global `a`. ¿Por qué? Porque en este caso, el enlace por defecto para `this` se aplica a la llamada de la función, y así apunta `this` al objeto global.

¿Cómo sabemos que la regla de vinculación predeterminada se aplica aquí? Examinamos el sitio de llamada para ver cómo se llama `foo()`. En nuestro fragmento, `foo()` se llama con una referencia de función simple, no decorada. Ninguna de las otras reglas que demostraremos se aplicará aquí, por lo que se aplica la vinculación por defecto.

Si el `strict mode` está en efecto, el objeto global no es elegible para el enlace predeterminado, por lo que `this` se establece como `undefined`.

```js
function foo() {
    "use strict";

    console.log( this.a );
}

var a = 2;

foo(); // TypeError: `this` is `undefined`
```

Un detalle sutil pero importante es: aunque el conjunto de reglas de vinculación de `this` se basan totalmente en el call-site, el objeto global sólo es elegible para el enlace por defecto si el contenido de `foo()` no se ejecuta en modo estricto; El estado de modo estricto del sitio de llamada de `foo()` es irrelevante.

```js
function foo() {
    console.log( this.a );
}

var a = 2;

(function(){
    "use strict";

    foo(); // 2
})();
```

Nota: La mezcla intencional de modo estricto y modo no estricto en su propio código generalmente es mal visto. Su programa entero debe ser probablemente Estricto o no Estricto. Sin embargo, a veces se incluye una biblioteca de terceros que tiene diferente Strict'ness en su propio código, por lo que hay que tener cuidado con estos sutiles detalles de compatibilidad.

### Vinculación implícita

Otra regla a considerar es: el sitio de llamada tiene un objeto de contexto, también conocido como un objeto propietario o que contiene, aunque estos términos alternativos podrían ser un poco engañosos.

Considere:

```js
function foo() {
    console.log( this.a );
}

var obj = {
    a: 2,
    foo: foo
};

obj.foo(); // 2
```

En primer lugar, observe la manera en que `foo()` se declara y luego se agrega como una propiedad de referencia en `obj`. Independientemente de si `foo()` se declara inicialmente en `obj`, o se agrega como una referencia posterior \(como muestra este fragmento\), en ninguno de los casos la función es realmente una "propiedad" o está "contenida" por el objeto `obj`.

Sin embargo, el sitio de llamada utiliza el contexto `obj` para referenciar la función, por lo que podría decirse que el objeto `obj` "posee" o "contiene" la referencia de función en el momento en que se llama a la función.

Lo que usted decida para llamar a este patrón, en el punto que `foo()` se llama, es precedido por una referencia de objeto a `obj`. Cuando hay un objeto de contexto para una referencia de función, la regla de vinculación implícita dice que es el objeto que se debe utilizar para la llamada de función de la vinculación de `this`.

Debido a que `obj` es lo mismo que `this` para la llamada `foo()`, `this.a` es sinónimo de `obj.a`.

Sólo el nivel superior / último de una cadena de referencia de propiedad de objeto es importante para el sitio de llamada. Por ejemplo:

```js
function foo() {
    console.log( this.a );
}

var obj2 = {
    a: 42,
    foo: foo
};

var obj1 = {
    a: 2,
    obj2: obj2
};

obj1.obj2.foo(); // 42
```

#### Implícitamente perdido

Una de las frustraciones más comunes que crea la vinculación de `this` es cuando una función implícitamente vinculada pierde esa vinculación, lo que generalmente significa que se vuelve a la vinculación por defecto, ya sea del objeto global o `undefined`, dependiendo del `strict mode`.

Considere:

```js
function foo() {
    console.log( this.a );
}

var obj = {
    a: 2,
    foo: foo
};

var bar = obj.foo; // function reference/alias!

var a = "oops, global"; // `a` also property on global object

bar(); // "oops, global"
```

A pesar de que `bar` parece ser una referencia a `obj.foo`, de hecho, es realmente sólo otra referencia a la propia `foo`. Además, el call-site es lo que importa, y el call-site es `bar()`, que es una llamada simple, no decorada y por lo tanto se aplica el enlace por defecto.

La forma más sutil, más común y más inesperada en que esto ocurre es cuando consideramos pasar una función de devolución de llamada:

```js
function foo() {
    console.log( this.a );
}

function doFoo(fn) {
    // `fn` is just another reference to `foo`

    fn(); // <-- call-site!
}

var obj = {
    a: 2,
    foo: foo
};

var a = "oops, global"; // `a` also property on global object

doFoo( obj.foo ); // "oops, global"
```

El paso de parámetros es simplemente en una asignación implícita, y puesto que estamos pasando una función, es una asignación implícita de referencia, de modo que el resultado final es el mismo que el fragmento anterior.

¿Qué pasa si la función a la que está pasando su devolución de llamada no es suya, sino integrada en el lenguaje? No hay diferencia, da el mismo resultado.

```js
function foo() {
    console.log( this.a );
}

var obj = {
    a: 2,
    foo: foo
};

var a = "oops, global"; // `a` also property on global object

setTimeout( obj.foo, 100 ); // "oops, global"
```

Piense en esta pseudo-implementación teórica cruda de `setTimeout()` proporcionada como un built-in del entorno JavaScript:

```js
function setTimeout(fn,delay) {
    // wait (somehow) for `delay` milliseconds
    fn(); // <-- call-site!
}
```

Es bastante común que nuestras devoluciones de funciones pierdan su vinculación `this`, como acabamos de ver. Pero otra manera en que `this` puede sorprendernos es cuando la función que hemos pasado a nuestra devolución de llamada a intencionalmente cambia `this` para la llamada. Los manejadores de eventos en las bibliotecas de JavaScript populares son muy aficionados a forzar su devolución de llamada a tener un `this`  al que apunta, por ejemplo, al elemento DOM que activó el evento. Mientras que a veces puede ser útil, otras veces puede ser francamente exasperante. Lamentablemente, estas herramientas rara vez le permiten elegir.

De cualquier manera `this` se cambia inesperadamente, usted no está realmente en control de cómo su referencia de función de devolución de llamada se ejecutará, así que usted no tiene ninguna manera \(aún\) de controlar el call-site para dar su vinculación deseada. Veremos pronto una manera de "arreglar" ese problema arreglando `this`.

### Vinculación explícita

Con la vinculación implícita como acabamos de ver, tuvimos que mutar el objeto en cuestión para incluir una referencia sobre sí misma a la función, y usar esta referencia de función de propiedad para indirectamente \(implícitamente\) vincular `this` con el objeto.

¿Pero, qué pasa si usted quiere forzar una llamada de la función para utilizar un objeto particular para este enlace, sin poner una referencia de la función de la característica en el objeto?

Todas las funciones del lenguaje tienen algunas utilidades a su disposición \(a través de su \[\[Prototype\]\]\) - más sobre esto más adelante\) que pueden ser útiles para esta tarea. Específicamente, las funciones tienen métodos de `call(..)` y de `apply(..)`. Técnicamente, los ambientes de host JavaScript a veces proporcionan funciones que son lo suficientemente especiales \(una forma amable de decirlo!\) Que no tienen dicha funcionalidad. Pero son pocos. La gran mayoría de las funciones proporcionadas, y ciertamente todas las funciones que va a crear, tienen acceso a `call(..)` y  `apply(..)`.

¿Cómo funcionan estas utilidades? Ambos toman, como su primer parámetro, un objeto para usar para `this`, y luego invocan la función con el `this` especificado. Puesto que usted está indicando directamente lo que usted quiere que `this` sea, lo llamamos enlace explícito.

Considere:

```js
function foo() {
    console.log( this.a );
}

var obj = {
    a: 2
};

foo.call( obj ); // 2
```

Invocando `foo` con una vinculación explícita por `foo.call(..)` nos permite forzar a su `this` a ser `obj`.

Si pasa un valor primitivo simple \(de tipo `string`, `boolean` o `number`\) como este enlace, el valor primitivo se envuelve en su objeto-forma \(`new String(..)`, `new Boolean(..)` o `new Number(..)`, respectivamente\). Esto se refiere a menudo como "boxing".

**Nota**: Con respecto a la vinculación de `this`, el `call(..)` y  `apply(..)` son idénticas. Ellos se comportan de manera diferente con sus parámetros adicionales, pero eso no es algo que nos importa en este momento.

Desafortunadamente, la vinculación explícita por sí sola todavía no ofrece ninguna solución al problema mencionado anteriormente, de una función "perder" su intención de vinculación `this`, o simplemente tenerla pavimentada por un framework, etc.

#### Vinculación dura \(Hard Binding\)

Pero un patrón de variación en torno a la vinculación explícita realmente hace el truco. Considere:

```js
function foo() {
    console.log( this.a );
}

var obj = {
    a: 2
};

var bar = function() {
    foo.call( obj );
};

bar(); // 2
setTimeout( bar, 100 ); // 2

// `bar` hard binds `foo`'s `this` to `obj`
// so that it cannot be overriden
bar.call( window ); // 2
```

Veamos cómo funciona esta variación. Creamos una función `bar()`que, internamente, llama manualmente a `foo.call(obj)`, invocando forzosamente `foo` con la vinculación `obj` para `this`. No importa cómo invoque más tarde la función `bar()`, siempre se invocará manualmente `foo` con `obj`. Esta vinculación es tanto explícita como fuerte \(hard\), por lo que lo llamamos vinculación dura \(hard binding\).

La forma más típica de envolver una función con una vinculación dura crea un paso a través de cualquier argumento pasado y cualquier valor de retorno recibido:

```js
function foo(something) {
    console.log( this.a, something );
    return this.a + something;
}

var obj = {
    a: 2
};

var bar = function() {
    return foo.apply( obj, arguments );
};

var b = bar( 3 ); // 2 3
console.log( b ); // 5
```

Otra forma de expresar este patrón es crear un ayudante reutilizable:

```js
function foo(something) {
    console.log( this.a, something );
    return this.a + something;
}

// simple `bind` helper
function bind(fn, obj) {
    return function() {
        return fn.apply( obj, arguments );
    };
}

var obj = {
    a: 2
};

var bar = bind( foo, obj );

var b = bar( 3 ); // 2 3
console.log( b ); // 5
```

Dado que la vinculación dura es un patrón tan común, se proporciona con una utilidad integrada como ES5: `Function.prototype.bind`, y se utiliza de esta manera:

```js
unction foo(something) {
    console.log( this.a, something );
    return this.a + something;
}

var obj = {
    a: 2
};

var bar = foo.bind( obj );

var b = bar( 3 ); // 2 3
console.log( b ); // 5
```

`bind(..)` devuelve una nueva función que está codificada para llamar a la función original con  el contexto `this` como se especificó.

**Nota**: A partir de ES6, la función hard-bound producida por `bind(..)` tiene una propiedad `.name` que deriva de la función de destino original. Por ejemplo: `bar = foo.bind(..)` debe tener un valor `bar.name` de "`bound foo`", que es el nombre de la llamada a la función que debe aparecer en un stack trace.

#### "Contextos" de API Call

Muchas funciones de las bibliotecas, y de hecho muchas nuevas funciones incorporadas en el lenguaje JavaScript y el entorno del host, proporcionan un parámetro opcional, generalmente llamado "context", que está diseñado como una solución para no tener que usar `bind(..)` Para asegurar que su función de devolución de llamada utilice un `this` en particular.

Por ejemplo:

```js
function foo(el) {
	console.log( el, this.id );
}

var obj = {
	id: "awesome"
};

// use `obj` como `this` para la llamada `foo(..)`
[1, 2, 3].forEach( foo, obj ); // 1 awesome  2 awesome  3 awesome
```

Internamente, estas diversas funciones casi seguramente usan una vinculación explícito vía  `call(..)` o  `apply(..)`, ahorrándole el problema.

### `new` Binding

La cuarta y última regla para la vinculación `this` nos obliga a repensar un concepto erróneo muy común sobre las funciones y los objetos en JavaScript.

En los lenguajes tradicionales orientados a clases, los "constructores" son métodos especiales asociados a las clases, que cuando la clase se instancia con un operador `new`, se llama al constructor de esa clase. Esto normalmente se parece a algo así:

```js
something = new MyClass(..);
```

JavaScript tiene un operador `new`, y el patrón de código para usarlo parece idéntico a lo que vemos en esos lenguajes orientados a clases; La mayoría de los desarrolladores asumen que el mecanismo de JavaScript está haciendo algo similar. Sin embargo, realmente no hay ninguna conexión a la funcionalidad orientada a clases implicada por el uso de `new` en JS.

En primer lugar, vamos a volver a definir lo que es un "constructor" en JavaScript. En JS, los constructores son sólo funciones que pasan a ser llamadas con el operador `new` delante de ellas. No están unidos a las clases, ni están instanciando una clase. Ni siquiera son tipos especiales de funciones. Son sólo funciones regulares que, en esencia, son secuestradas por el uso de `new` en su invocación.

Por ejemplo, la función `Number(..)` que actúa como constructor, citando desde la especificación ES5.1:

> 15.7.2 El Constructor de Números
>
> Cuando Number se llama como parte de una expresión `new`, es un constructor: inicializa el objeto recién creado.

Por lo tanto, casi cualquier función, incluyendo las funciones de objetos integradas como `Number(..)` \(véase el capítulo 3\) se puede llamar con `new` delante de ella, y hace que la función llamada sea una llamada al constructor. Esta es una distinción importante pero sutil: realmente no hay tal cosa como "funciones constructoras", sino más bien llamadas de construcción de funciones.

Cuando se invoca una función con `new` delante de ella, también conocida como llamada del constructor, se ejecutan automáticamente las siguientes acciones:

1. Un nuevo objeto se crea \(aka, construido\) en el aire
2. El objeto de nueva construcción es \[\[Prototype\]\] - linked
3. El objeto de nueva construcción se establece como el enlace de esa llamada de función
4. A menos que la función devuelva su propio **objeto** alternativo, la llamada de función invocada automáticamente devolverá el objeto recién construido.

Los pasos 1, 3 y 4 se aplican a nuestra discusión actual. Vamos a saltar el paso 2 por ahora y volver a él en el capítulo 5.

Considere este código:

```js
function foo(a) {
	this.a = a;
}

var bar = new foo( 2 );
console.log( bar.a ); // 2
```

Al llamar a `foo(...)` con un `new` delante de él, hemos construido un nuevo objeto y establecemos ese nuevo objeto como el de la llamada de `foo(..)`. Por tanto `new` es la manera final en que una llamada a la función `this` puede ser vinculada. Llamaremos a esto nueva vinculación \(new banding\).



