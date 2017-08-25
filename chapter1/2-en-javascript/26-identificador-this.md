# 2.6 Identificador `This`

Otro concepto muy comúnmente mal entendido en JavaScript es el identificador `this`. Una vez más, hay un par de capítulos sobre él de esta serie, así que aquí vamos a introducir brevemente el concepto.

Aunque a menudo parece que `this` está relacionado con "patrones orientados a objetos", en JS este es un mecanismo diferente.

Si una función tiene una referencia `this` dentro de ella, esa referencia `this` normalmente apunta a un `object`. Pero el `object` al que apunta depende de cómo se llamó la función.

Es importante darse cuenta de que `this` no se refiere a la función en sí, y ése es el error más común.

He aquí una ilustración rápida:

```js
function foo() {
	console.log( this.bar );
}

var bar = "global";

var obj1 = {
	bar: "obj1",
	foo: foo
};

var obj2 = {
	bar: "obj2"
};

// --------

foo();				// "global"
obj1.foo();			// "obj1"
foo.call( obj2 );		// "obj2"
new foo();			// undefined
```

Hay cuatro reglas para cómo se establece `this`, y se muestran en las cuatro últimas líneas de ese fragmento.

1. `foo()` termina configurando `this` para el objeto global en modo no estricto - en modo estricto, esto sería `undefined` y obtendría un error en el acceso a la propiedad `bar` - por lo que "`global`" es el valor encontrado para este `.bar`.
2. `obj1.foo()` establece `this` en el object `obj1`.
3. `foo.call(obj2)` establece `this` en el objeto `obj2`.
4. `new foo()` establece `this` en un nuevo objeto vacío.

En pocas palabras: para entender a qué se refiere `this`, hay que examinar cómo se llamó la función en cuestión. Será una de esas cuatro maneras que acabamos de mostrar, y eso responderá a lo que es `this`.

Nota: Para obtener más información al respecto, consulte los capítulos 1 y 2 del título This y Objetos Prototipos de esta serie.





