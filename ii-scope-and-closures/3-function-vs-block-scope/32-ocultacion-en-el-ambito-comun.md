# 3.2 Ocultación en el ámbito común

La forma tradicional de pensar acerca de las funciones es que declares una función y luego agregas código dentro de ella. Pero el pensamiento inverso es igualmente poderoso y útil: toma cualquier sección arbitraria de código que hayas escrito, y envuelve una declaración de función alrededor de ella, que en efecto "oculta" el código.

El resultado práctico es crear una burbuja de alcance alrededor del código en cuestión, lo que significa que cualquier declaración \(variable o función\) en ese código ahora estará vinculada al ámbito de la nueva función de empaquetado, en lugar del ámbito de inclusión anterior. En otras palabras, puede "ocultar" variables y funciones encerrándolas en el ámbito de una función.

¿Por qué las variables y funciones "ocultas" serían una técnica útil?

Hay una variedad de razones que motivan este ocultamiento basado en el alcance. Ellos tienden a surgir a partir del principio de diseño de software "Principio de Menor Privilegio", también llamado a veces "Menos Autoridad" o "Menos Exposición". Este principio establece que en el diseño de software, como la API para un módulo/objeto, debe exponer sólo lo que es mínimamente necesario y "ocultar" todo lo demás.

Este principio se extiende a la elección de qué ámbito debe contener variables y funciones. Si todas las variables y funciones estuvieran en el ámbito global, obviamente serían accesibles para cualquier ámbito anidado. Pero esto violaría el principio de "Menos ..." en el que usted está \(probablemente\) exponiendo muchas variables o funciones que de otra manera debería mantener privadas, ya que el uso apropiado del código desalentaría el acceso a esas variables/funciones.

Por ejemplo:

```js
function doSomething(a) {
	b = a + doSomethingElse( a * 2 );

	console.log( b * 3 );
}

function doSomethingElse(a) {
	return a - 1;
}

var b;

doSomething( 2 ); // 15
```

En este fragmento, la variable `b` y la función `doSomethingElse(..)` son probablemente detalles "privados" de cómo `doSomething(..)` hace su trabajo. Dando el ámbito de acceso "acceso" a `b` y `doSomethingElse(..)` no sólo es innecesario, sino también posiblemente "peligroso", ya que puede ser utilizado de manera inesperada, intencionalmente o no, y esto puede violar las suposiciones de pre-condición de `doSomething(..)`.

Un diseño más "apropiado" ocultaría estos detalles privados dentro del alcance de `doSomething(...)`, tales como:

```js
function doSomething(a) {
	function doSomethingElse(a) {
		return a - 1;
	}

	var b;

	b = a + doSomethingElse( a * 2 );

	console.log( b * 3 );
}

doSomething( 2 ); // 15
```

Ahora, `b` y `doSomethingElse(..)` no son accesibles a ninguna influencia exterior, como cuando estaban controladas sólo por `doSomething(..)`. La funcionalidad y el resultado final no se han visto afectados, pero el diseño mantiene los detalles privados, lo que suele considerarse un mejor software.

### Evitar colisiones

Otro beneficio de "ocultar" variables y funciones dentro de un ámbito es evitar la colisión no deseada entre dos identificadores diferentes con el mismo nombre pero con diferentes usos. Los resultados de la colisión a menudo producen una sobrescritura inesperada de los valores.

Por ejemplo:

```js
function foo() {
	function bar(a) {
		i = 3; // Cambiar el `i` en el ámbito de inclusión for-loop
		console.log( a + i );
	}

	for (var i=0; i<10; i++) {
		bar( i * 2 ); // Oops, bucle infinito!
	}
}

foo();
```

La asignación` i = 3` dentro de `bar(..)` sobrescribe, inesperadamente, el `i` que se declaró en `foo(..)` en el bucle `for`. En este caso, resultará en un bucle infinito, porque `i` se establece en un valor fijo de `3` y que permanecerá para siempre `<10`.

La asignación dentro de la `bar(..)` necesita declarar una variable local para usar, independientemente del nombre de identificador elegido. `var i = 3;` solucionaría el problema \(y crearía la declaración de "variable sombreada" anteriormente mencionada para `i`\). Una opción adicional, no alternativa, es escoger otro nombre de identificador, tal como `var j = 3` ;. Sin embargo, su diseño de software, naturalmente, puede llamar el mismo nombre de identificador, por lo que la utilización de alcance para "ocultar" su declaración interna es la mejor/única opción en ese caso.

#### Global "Namespaces"

Un ejemplo particularmente fuerte de colisión variable \(probable\) ocurre en el ámbito global. Las bibliotecas múltiples cargadas en su programa pueden chocar muy fácilmente entre sí si no ocultan correctamente sus funciones y variables internas/privadas.

Dichas bibliotecas normalmente crearán una declaración de variable única, a menudo un objeto, con un nombre suficientemente único, en el ámbito global. Este objeto se utiliza a continuación como un "espacio de nombres" para esa biblioteca, donde todas las exposiciones específicas de la funcionalidad se hacen como propiedades fuera de ese objeto \(espacio de nombres\), en lugar de identificadores de nivel superior de ámbito léxico por sí mismos.

Por ejemplo:

```js
var MyReallyCoolLibrary = {
	awesome: "stuff",
	doSomething: function() {
		// ...
	},
	doAnotherThing: function() {
		// ...
	}
};
```

#### Gestión de módulos

Otra opción para evitar colisiones es el más moderno enfoque de "módulo", utilizando cualquiera de los distintos administradores de dependencia. Utilizando estas herramientas, ninguna biblioteca agrega identificadores al ámbito global, sino que se requiere que su identificador\(es\) sean explícitamente importados a otro ámbito específico mediante el uso de los diversos mecanismos del administrador de dependencias.

Debe observarse que estas herramientas no poseen funcionalidad "mágica" que está exenta de reglas de alcance léxico. Simplemente usan las reglas de alcance como se explica aquí para hacer cumplir que no se inyectan identificadores en ningún ámbito compartido y, en cambio, se mantienen en ámbitos privados no susceptibles a colisiones, lo que evita colisiones de alcance accidental.

Como tal, puede su código defensivamente, lograr los mismos resultados que los hacen los administradores de dependencias  sin necesidad de utilizarlos, si así lo desea. Consulte el Capítulo 5 para obtener más información sobre el patrón del módulo.

