# 5.2 Nitty Gritty

OK, suficiente hipérbole y referencias de películas desvergonzadas.

Aquí está una definición de lo que usted necesita saber para entender y reconocer los closures:

> El closure es cuando una función es capaz de recordar y acceder a su ámbito léxico incluso cuando esa función está ejecutándose fuera de su ámbito léxico.

Vamos a ver algo de código para ilustrar esa definición.

```js
function foo() {
    var a = 2;

    function bar() {
        console.log( a ); // 2
    }

    bar();
}

foo();
```

Este código debe parecer familiar a nuestras discusiones de Nested Scope. La funcion `bar()` tiene acceso a la variable `a` en el ámbito externo de inclusión debido a las reglas de búsqueda de alcance léxico \(en este caso, es una referencia de referencia RHS\).

¿Esto este un "closure"?

Bueno, técnicamente ... tal vez. Pero con nuestra definición de lo que necesitas saber arriba ... no exactamente. Creo que la manera más precisa de explicar la forma en que `bar()` hace referencia a `a` es a través de las reglas de búsqueda de alcance léxico, y esas reglas son sólo \(una parte importante!\) de lo que es el closure.

Desde una perspectiva puramente académica, lo que se dice del fragmento anterior es que la función `bar()` tiene un closure sobre el alcance de `foo()` \(e incluso sobre el resto de los ámbitos a los que tiene acceso, como el alcance global en nuestro caso\). Se dice que `bar()` se **cierra \(closes\)** sobre el alcance de `foo()`. ¿Por qué? porque `bar()` aparece anidado dentro de `foo()`. Llano y simple.

Pero, el closure definido de esta manera no es directamente observable, ni vemos el cierre ejercido en ese fragmento. Vemos claramente el alcance léxico, pero el closure sigue siendo una especie de misteriosa sombra detrás del código.

Consideremos entonces el código que pone el closure a plena luz:

```js
function foo() {
	var a = 2;

	function bar() {
		console.log( a );
	}

	return bar;
}

var baz = foo();

baz(); // 2 -- Whoa, el cierre se acaba de observar, hombre!.
```

La funcion `bar()` tiene acceso de alcance léxico al ámbito interno de `foo()`. Pero entonces tomamos `bar()`, la función en sí, y la pasamos como un valor. En este caso, retornamos el objeto de función propiamente dicho que hace referencia a `bar`.

Después de ejecutar `foo()`, asignamos el valor que retorno \(nuestra función interna `bar()`\) a una variable llamada `baz`, y luego invocamos `baz()`, que por supuesto está invocando nuestra función interna `bar()`, sólo por una referencia de identificador diferente.

`bar()` se ejecuta, por supuesto. Pero en este caso, se ejecuta fuera de su ámbito léxico declarado.

Después de ejecutado `foo()` , normalmente esperamos que la totalidad del alcance interno de `foo()` se vaya, porque sabemos que el motor emplea un recolector de basura \(Garnage Collector\) que viene y libera memoria una vez que ya no está en uso. Puesto que parecería que el contenido de `foo()` ya no está en uso, parecería natural que lo consideren como desaparecido.

Pero la "magia" de los closures no deja que esto suceda. Ese alcance interior está, de hecho, todavía "en uso", y por lo tanto no desaparece. **¿Quién lo usa? La funcion `bar()` en sí.**

En virtud de donde fue declarado, `bar()` tiene un closure de alcance léxico sobre ese alcance interno de `foo()`, que mantiene ese alcance vivo para que `bar()` le haga una referencia en cualquier momento posterior.

**`bar()` todavía tiene una referencia a ese ámbito, y esa referencia se llama closure.**

Por lo tanto, unos pocos microsegundos más tarde, cuando se invoca la variable `baz` \(invocando la función interna inicialmente etiquetada `bar`\), tiene debidamente acceso al ámbito lexical de autor-tiempo, por lo que puede acceder a la variable tal como esperábamos.

La función se invoca bien fuera de su alcance léxico de autor-tiempo. El closure permite que la función siga teniendo acceso al ámbito léxico en el que se definió en la hora de autor.

Por supuesto, cualquiera de las varias maneras en que las funciones pueden ser pasadas alrededor como valores, y de hecho invocadas en otros lugares, son todos ejemplos de observación/ejercicio de closures.

```js
function foo() {
	var a = 2;

	function baz() {
		console.log( a ); // 2
	}

	bar( baz );
}

function bar(fn) {
	fn(); // Mira ma, vi el closure!
}
```

Pasamos la función interna `baz` a la funcion `bar()`, y llamamos a esa función interna \(etiquetada `fn` ahora\), y cuando lo hacemos, su closure sobre el alcance interno de `foo()` se observa, accediendo a `a`.

Estas pasadas de funciones también pueden ser indirectas.

```js
var fn;

function foo() {
	var a = 2;

	function baz() {
		console.log( a );
	}

	fn = baz; // asigna `baz` a una variable global
}

function bar() {
	fn(); // Mira ma, vi el closure!
}

foo();

bar(); // 2
```

Cualquiera que sea la facilidad que utilicemos para _**transportar**_ una función interna fuera de su ámbito léxico, mantendrá una referencia de alcance a donde originalmente fue declarada, y dondequiera que la ejecute, se ejercerá ese closure.

