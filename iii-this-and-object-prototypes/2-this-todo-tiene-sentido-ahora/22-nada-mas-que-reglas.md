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

