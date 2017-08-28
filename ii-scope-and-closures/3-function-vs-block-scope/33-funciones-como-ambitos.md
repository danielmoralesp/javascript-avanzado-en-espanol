# 3.3 Funciones como ámbitos

Hemos visto que podemos tomar cualquier fragmento de código y envolver una función alrededor de él, y que efectivamente "oculta" cualquier declaración de función o variable encerrada desde el ámbito externo dentro del ámbito interno de esa función.

Por ejemplo:

```js
var a = 2;

function foo() { // <-- insert this

    var a = 3;
    console.log( a ); // 3

} // <-- and this
foo(); // <-- and this

console.log( a ); // 2
```

Si bien esta técnica "funciona", no es necesariamente ideal. Hay algunos problemas que introduce. El primero es que tenemos que declarar una función nombrada `foo()`, lo que significa que el nombre del identificador `foo` mismo "contamina" el ámbito de inclusión \(global, en este caso\). También tenemos que llamar explícitamente a la función por nombre \(`foo()`\) para que el código envuelto en realidad se ejecute.

Sería más ideal si la función no necesitaba un nombre \(o, más bien, el nombre no contamine el ámbito de inclusión\) y si la función podría ejecutarse automáticamente.

Afortunadamente, JavaScript ofrece una solución a ambos problemas.

```js
var a = 2;

(function foo(){ // <-- insert this

	var a = 3;
	console.log( a ); // 3

})(); // <-- and this

console.log( a ); // 2
```

Vamos a desglosar lo que está sucediendo aquí.

En primer lugar, observe que la sentencia de la función wrapping comienza con `(function...`  \(con paréntesis al inicio\) en lugar de simplemente `function...`. Si bien esto puede parecer un detalle menor, en realidad es un cambio importante. En lugar de tratar la función como una declaración estándar, la función se trata como una función-expresión.

**Nota**: La forma más fácil de distinguir entre declaración y expresión es la posición de la palabra "`function`" en la sentencia \(no sólo una línea, sino una declaración distinta\). Si "`function`" es la primera parte de la declaración, entonces es una declaración de función. De lo contrario, es una expresión de función.

La diferencia clave que podemos observar aquí entre una declaración de función y una expresión de función se refiere a donde su nombre está enlazado como un identificador.

Compare los dos fragmentos anteriores. En el primer fragmento, el nombre `foo` está enlazado en el ámbito de inclusión, y lo llamamos directamente con `foo()`. En el segundo snippet, el nombre `foo` no está enlazado en el ámbito de inclusión, sino que está limitado sólo dentro de su propia función.

En otras palabras, `(function foo() {..})` como una expresión significa que el identificador `foo` se encuentra sólo en el ámbito donde se indican los tres puntos `...`, no en el ámbito externo. Ocultar el nombre `foo` dentro de sí significa que no contamina el ámbito de inclusión innecesariamente.

### Anónimo vs. Nombrado

Probablemente esté más familiarizado con las expresiones de función como parámetros de devolución de llamada, como:

```js
setTimeout( function(){
	console.log("I waited 1 second!");
}, 1000 );
```

Esto se denomina "expresión de función anónima", porque `function()...` no tiene identificador de nombre en ella.

Las expresiones de función pueden ser anónimas, pero las declaraciones de funciones no pueden omitir el nombre, que sería una gramática ilegal en JS.

Las expresiones de función anónimas son rápidas y fáciles de escribir, y muchas bibliotecas y herramientas tienden a fomentar este estilo de código idiomático.

Sin embargo, tienen varios inconvenientes a considerar:

1. Las funciones anónimas no tienen ningún nombre útil para mostrar en lo stack trace \(errores\), lo que puede dificultar la depuración.
2. Sin un nombre, si la función necesita referirse a sí misma, para la recursión, etc., la referencia **deprecated** `arguments.callee` es desafortunadamente requerida. Otro ejemplo de la necesidad de autorreferencia es cuando una función de controlador de eventos desea desvincularse después de que se activa.
3. Las funciones anónimas omiten un nombre que es a menudo útil en proporcionar código más legible/comprensible. Un nombre descriptivo ayuda a auto-documentar el código en cuestión.

Las expresiones de la función en línea son poderosas y útiles - la cuestión de anónimo vs nombre no quita eso. Proporcionar un nombre para su expresión de función con bastante efectividad aborda todos estos draw-backs, pero no tiene desventajas tangibles. **La mejor práctica es nombrar siempre las expresiones de su función**:

```js
setTimeout( function timeoutHandler(){ // <-- Look, I have a name!
	console.log( "I waited 1 second!" );
}, 1000 );
```

### Invocando expresiones de función inmediatamente

```js
var a = 2;

(function foo(){

	var a = 3;
	console.log( a ); // 3

})();

console.log( a ); // 2
```

Ahora que tenemos una función como una expresión en virtud de envolverla en un par `( )`, podemos ejecutar esa función añadiendo otro `( )` al final, como `(función foo() {..}) ()`. El primer par enclosing `( )` hace que la función sea una expresión, y la segunda `( )` que ejecute la función.

Este patrón es tan común, hace unos años la comunidad acordó un término para ello: `IIFE`, que significa expresión de función inmediatamente invocada.

Por supuesto, los `IIFE` no necesitan nombres, necesariamente - la forma más común de IIFE es usar una expresión de función anónima. Aunque ciertamente menos común, nombrar un IIFE tiene todos los beneficios antes mencionados sobre expresiones de función anónimas, por lo que es una buena práctica por adoptar.

```js
var a = 2;

(function IIFE(){

	var a = 3;
	console.log( a ); // 3

})();

console.log( a ); // 2
```

Hay una ligera variación en el formulario tradicional IIFE, que algunos prefieren: `(function() {..}())`. Mira con atención para ver la diferencia. En la primera forma, la expresión de la función se envuelve en `( ),` y entonces se invoca con el par  `( )` que está en el exterior justo después de él. En la segunda forma, el par que invoca `( )` se mueve al interior del par envolvente exterior `( )`.

Estas dos formas son idénticas en funcionalidad. Es puramente una elección estilística que usted prefiera.

Otra variación en IIFE que es bastante común es usar el hecho de que son, de hecho, sólo llamadas de función, y pasarlas en argumento\(s\).

Por ejemplo:

```js
var a = 2;

(function IIFE( global ){

	var a = 3;
	console.log( a ); // 3
	console.log( global.a ); // 2

})( window );

console.log( a ); // 2
```

Pasamos en la referencia de objeto `window`, pero nombramos el parámetro `global`, de modo que tengamos una delineación estilística clara para referencias globales vs. no globales. Por supuesto, puede pasar cualquier cosa desde un ámbito incluido que desee, y puede nombrar el parámetro \(s\) cualquier cosa que se adapte a usted. Esto es en su mayoría sólo elección estilística.

Otra aplicación de este patrón aborda los concerns \(nicho menor\) de que el identificador indefinido por defecto podría tener su valor incorrectamente sobrescrito, causando resultados inesperados. Al nombrar un parámetro indefinido, pero sin pasar ningún valor para ese argumento, podemos garantizar que el identificador indefinido es de hecho el valor indefinido en un bloque de código:

```js
undefined = true; // El establecimiento de una mina de tierra para el otro código! ¡evitarlo!

(function IIFE( undefined ){

	var a;
	if (a === undefined) {
		console.log( "Undefined is safe here!" );
	}

})();
```

Otra variación del IIFE invierte el orden de las cosas, donde la función a ejecutar se da en segundo lugar, después de la invocación y los parámetros para pasarle. Este patrón se utiliza en el proyecto UMD \(Universal Module Definition\). Algunas personas lo encuentran un poco más limpio de entender, aunque es un poco más detallado.

```js
var a = 2;

(function IIFE( def ){
	def( window );
})(function def( global ){

	var a = 3;
	console.log( a ); // 3
	console.log( global.a ); // 2

});
```

La expresión de la función `def` se define en la segunda mitad del fragmento y se pasa como parámetro \(también llamado `def`\) a la función IIFE definida en la primera mitad del fragmento. Finalmente, se invoca el parámetro `def` \(la función\), pasando `window`  como parámetro global.















