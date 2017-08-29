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







