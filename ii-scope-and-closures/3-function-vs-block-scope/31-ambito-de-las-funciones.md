# 3.1 Ámbito de las funciones

La respuesta más común a esas preguntas es que JavaScript tiene un alcance/ámbito/scope basado en funciones. Es decir, cada función que declara crea una nueva burbuja para sí misma, pero ninguna otra estructura crea sus propias burbujas de alcance. Como veremos más adelante, esto no es del todo cierto.

Pero primero, vamos a explorar el alcance de la función y sus implicaciones.

Considere este código:

```js
function foo(a) {
	var b = 2;

	// some code

	function bar() {
		// ...
	}

	// more code

	var c = 3;
}
```

En este fragmento, la burbuja de alcance para `foo(..)` incluye los identificadores `a`, `b`, `c` y `bar`. No importa dónde en el ámbito de una declaración aparezca, la variable o función pertenece a la burbuja que contiene el ámbito, independientemente. Vamos a explorar cómo funciona eso exactamente en el próximo capítulo.

`bar(..)` tiene su propia burbuja de alcance. Lo mismo ocurre con el ámbito global, que tiene sólo un identificador adjunto a él: `foo`.

Debido a que `a`, `b`, `c`, y `bar`  pertenecen todos a la burbuja de alcance de `foo(..)`, no son accesibles fuera de `foo(..)`. Es decir, el código siguiente resultaría en errores `ReferenceError`, ya que los identificadores no están disponibles para el ámbito global:

```js
bar(); // fails

console.log( a, b, c ); // all 3 fail
```

Sin embargo, todos estos identificadores \(`a`, `b`, `c`, `foo` y `bar`\) son accesibles dentro de `foo(..)`, y de hecho también disponible dentro de `bar(..)` \(suponiendo que no hay declaraciones de identificador de sombra dentro de `bar(..)`\).

El ámbito de la función fomenta la idea de que todas las variables pertenecen a la función y pueden utilizarse y reutilizarse a lo largo de toda la función \(incluso accesibles a los ámbitos anidados\). Este enfoque de diseño puede ser muy útil, y ciertamente puede hacer pleno uso de la naturaleza "dinámica" de las variables JavaScript para asumir valores de diferentes tipos según sea necesario.

Por otro lado, si no se toman precauciones cuidadosas, las variables existentes a través de la totalidad de un ámbito puede conducir a algunas trampas inesperadas.

