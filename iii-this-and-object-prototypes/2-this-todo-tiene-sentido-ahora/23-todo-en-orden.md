# 2.3 Todo en orden

Por lo tanto, ahora hemos descubierto las 4 reglas para vincular `this` en las llamadas de función. Todo lo que necesita hacer es encontrar el sitio de llamada e inspeccionarlo para ver qué regla se aplica. Pero, ¿qué pasa si el sitio de llamada tiene múltiples reglas elegibles? Debe haber un orden de precedencia a estas reglas, por lo que demostraremos a continuación cuál es el orden para aplicar las reglas.

Debe quedar claro que la vinculación por defecto es la regla de prioridad más baja de las 4. Por lo tanto, vamos a dejarla de lado.

¿Cuál es más precedente, vinculación implícita o vinculación explícita? Vamos a probarlo:

```js
function foo() {
	console.log( this.a );
}

var obj1 = {
	a: 2,
	foo: foo
};

var obj2 = {
	a: 3,
	foo: foo
};

obj1.foo(); // 2
obj2.foo(); // 3

obj1.foo.call( obj2 ); // 3
obj2.foo.call( obj1 ); // 2
```



