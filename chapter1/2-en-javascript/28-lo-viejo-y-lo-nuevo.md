# 2.8 Lo Viejo y Lo Nuevo

Algunas de las características de JS que ya hemos cubierto, y ciertamente muchas de las características cubiertas en el resto de esta serie, son nuevas incorporaciones y no necesariamente estarán disponibles en navegadores antiguos. De hecho, algunas de las características más recientes de la especificación aún no están implementadas en ningún navegador estable.

Entonces, ¿qué hacemos con las cosas nuevas? ¿Sólo tenemos que esperar alrededor de unos años o décadas para que todos los viejos navegadores se desvanezcan en la oscuridad?

Esta es la forma en que la gente que piensa acerca de esta situación, pero en realidad no es un enfoque saludable para JS.

Hay dos técnicas principales que puede utilizar para "llevar" las cosas nuevas de JavaScript a los navegadores más antiguos: polyfilling y transpiling.

### Polyfilling

La palabra "polyfill" es un término inventado \(por Remy Sharp\) \([https://remysharp.com/2010/10/08/what-is-a-polyfill](https://remysharp.com/2010/10/08/what-is-a-polyfill)\) utilizado para referirse a tomar la definición de una característica más reciente y producir un pedazo de código que es equivalente al comportamiento, pero es capaz de ejecutarse en entornos JS más antiguos.

Por ejemplo, ES6 define una utilidad denominada `Number.isNaN(..)` para proporcionar una verificación exacta y sin errores en los valores `NaN`, despreciando la utilidad original `isNaN(..) `. Pero es fácil "polifilear" la utilidad para que pueda empezar a utilizarlo en su código, independientemente de si el usuario final está en un navegador ES6 o no.

Considere:

```js
if (!Number.isNaN) {
	Number.isNaN = function isNaN(x) {
		return x !== x;
	};
}
```

La instrucción `if` lo protege contra la aplicación de la definición de polyfill en los exploradores ES6 donde ya existirá. Si no está presente, definimos `Number.isNaN(..)`.

`Nota`: El chequeo que hacemos aquí se aprovecha de una peculiaridad con valores `NaN`, que es que son el único valor en todo el lenguaje que no es igual a sí mismo. Así que el valor `NaN` es el único que haría que `x !== x` sea `true`.

No todas las nuevas características son completamente polifilables. A veces la mayor parte del comportamiento puede ser polifileado, pero todavía hay pequeñas desviaciones. Usted debe ser muy, muy cuidadoso en la aplicación de un polyfill que haga usted mismo, para asegurarse de que se adhieren a la especificación tanto como sea posible.

O mejor aún, utilice un conjunto de polyfills ya probado en los que puede confiar, como los proporcionados por ES5-Shim \([https://github.com/es-shims/es5-shim](https://github.com/es-shims/es5-shim)\) y ES6-Shim \([https: // Github.com/es-shims/es6-shim](https: // Github.com/es-shims/es6-shim)\).

### Transpiling

No hay manera de polifilear nueva sintaxis que se ha agregado al lenguaje. La nueva sintaxis arrojaría un error en el antiguo motor JS como unrecognized/invalid.

Por lo tanto, la mejor opción es utilizar una herramienta que convierte su código más reciente en equivalentes de código antiguo. Este proceso es comúnmente llamado "transpiling", un término para transformar + compilar.

Esencialmente, su código fuente es creado en el nuevo formulario de sintaxis, pero lo que implementa en el navegador es el código transpilado en forma de sintaxis antigua. Por lo general, insertar el transpiler en su proceso de construcción, similar a su linter code o su minifier.

Usted podría preguntarse por qué me tomaría la molestia de escribir nueva sintaxis sólo para que se traslade lejos de código antiguo - ¿por qué no sólo escribir el código más antiguo directamente?

Hay varias razones importantes por las que debe preocuparse por el transpiling:

* La nueva sintaxis agregada al lenguaje está diseñada para hacer que su código sea más legible y mantenible. Los equivalentes más viejos son a menudo mucho más enrevesados. Debería preferir escribir una sintaxis nueva y más limpia, no sólo para usted sino para todos los demás miembros del equipo de desarrollo.
* Si transpila sólo para los navegadores más antiguos, pero sirve la nueva sintaxis a los navegadores más recientes, podrá aprovechar las optimizaciones del rendimiento del navegador con la nueva sintaxis. Esto también permite a los fabricantes del navegador tener más código del mundo real para probar sus implementaciones y optimizaciones.
* El uso de la nueva sintaxis anticipadamente permite que se pruebe de forma más robusta en el mundo real, lo que proporciona retroalimentación al comité JavaScript \(TC39\). Si los problemas se encuentran a tiempo, pueden ser cambiados/arreglados antes de que los errores de diseño de lenguaje se conviertan en permanentes.

Aquí está un ejemplo rápido de transpiling. ES6 agrega una característica llamada "valores de parámetros predeterminados". Se parece a esto:

```js
function foo(a = 2) {
	console.log( a );
}

foo();		// 2
foo( 42 );	// 42
```

Simple, ¿verdad? ¡Útil, también! Pero es una nueva sintaxis que no es válida en motores pre-ES6. Entonces, ¿qué hará un transpiler con ese código para que funcione en entornos más antiguos?

```js
function foo() {
	var a = arguments[0] !== (void 0) ? arguments[0] : 2;
	console.log( a );
}
```

Como puede ver, comprueba si el valor de los `arguments[0]` es `void 0` \(aka `undefined`\), y si es así, proporciona el valor predeterminado `2`; De lo contrario, asigna lo que se pasó.

Además de poder utilizar ahora la sintaxis más agradable incluso en los navegadores más antiguos, ver el código transpilado explica realmente el comportamiento previsto más claramente.

Es posible que no se haya dado cuenta sólo de mirar la versión de ES6 que `undefined` es el único valor que no se puede pasar explícitamente en un parámetro de valor predeterminado, pero el código transpilado lo hace mucho más claro.

El último detalle importante a destacar sobre los transpiladores es que ahora deben ser considerados como una parte estándar del ecosistema y proceso de desarrollo de JS. JS va a seguir evolucionando, mucho más rápidamente que antes, por lo que cada pocos meses se añadirá nueva sintaxis y nuevas características.

Si utiliza un transpilador de forma predeterminada, siempre podrá hacer que cambie a la sintaxis más reciente siempre que lo encuentre útil, en lugar de esperar años para que los navegadores de hoy se eliminen progresivamente.

Hay bastantes transpilers grandes para que usted elija de. Aquí hay algunas buenas opciones en el momento de escribir esto:

* Babel \([https://babeljs.io](https://babeljs.io/)\) \(formerly 6to5\): Transpila ES6+ dentro de ES5
* Traceur \([https://github.com/google/traceur-compiler](https://github.com/google/traceur-compiler)\): Transpila ES6, ES7, y más alla en ES5



