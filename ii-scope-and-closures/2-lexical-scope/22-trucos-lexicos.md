# 2.2 Trucos léxicos

Si el ámbito léxico está definido sólo por el lugar en el que se declara una función, que es enteramente una decisión de autor-tiempo, ¿cómo podría haber una forma de "modificar" \(aka, cheat\) el alcance léxico en tiempo de ejecución?

JavaScript tiene dos mecanismos. Ambos son igualmente mal vistos en la comunidad en general como malas prácticas para usar en su código. Pero los argumentos típicos en contra de ellos a menudo les falta el punto más importante: **engañar el alcance léxico conduce a un rendimiento más pobre**.

Antes de explicar el problema de rendimiento, veamos cómo funcionan estos dos mecanismos.

### `eval`

La función `eval(..)` en JavaScript toma una cadena como argumento y trata el contenido de la cadena como si realmente hubiera sido el código de autor en ese punto del programa. En otras palabras, puede generar código de forma programática dentro de su código creado y ejecutar el código generado como si hubiera estado allí en tiempo de autor.

Evaluando `eval(..)` \(juego de palabras a propósito\) en esa luz, debería estar claro cómo `eval(..)` le permite modificar el entorno del alcance léxico para hacer trampa y fingir que el código de autor-tiempo \(aka, lexical\) estaba allí todo el tiempo .

En las siguientes líneas de código después de que se haya ejecutado un `eval(..)`, el Motor no "sabrá" o "cuidará" que el código anterior en cuestión se haya interpretado dinámicamente y, por lo tanto, haya modificado el entorno del ámbito léxico. El Motor simplemente realizará sus análisis de alcance léxico como siempre lo hace.

Considere el siguiente código:

```js
function foo(str, a) {
	eval( str ); // cheating!
	console.log( a, b );
}

var b = 2;

foo( "var b = 3;", 1 ); // 1 3
```

La cadena "`var b = 3;`" es tratada en el punto de la llamada `eval(..)`, como código que estuvo allí todo el tiempo. Debido a que el código pasa a declarar una nueva variable `b`, modifica el ámbito léxico existente de `foo(..)`. De hecho, como se mencionó anteriormente, este código realmente crea la variable `b` dentro de `foo(..)` que sombrea la `b` que se declaró en el ámbito externo \(global\).

Cuando se produce la llamada `console.log(..)`, encuentra tanto `a` como `b` en el ámbito de `foo(..)`, y nunca encuentra la `b` externa. Así, imprimimos "`1 3`" en vez de "`1 2`" como habría sido normalmente el caso.

**Nota**: En este ejemplo, por simplicidad, la cadena de "código" que pasamos fue un literal fijo. Pero podría haber sido creada mediante programación agregando caracteres juntos en función de la lógica de su programa. `eval(..)` suele utilizarse para ejecutar código creado dinámicamente, ya que evaluar dinámicamente un código esencialmente estático a partir de una cadena literal no proporcionaría ningún beneficio real a la creación del código directamente.

De forma predeterminada, si una cadena de código que `eval(..)` ejecuta contiene una o más declaraciones \(variables o funciones\), esta acción modifica el ámbito léxico existente en el que reside el `eval(..)`. Técnicamente, `eval(..)` puede ser invocado "indirectamente", a través de varios trucos \(que va más allá de nuestra discusión aquí\), lo que hace que en lugar de ejecutar en el contexto del ámbito global, por lo tanto, modificarlo. Pero en cualquier caso, `eval(..)` puede en tiempo de ejecución modificar un ámbito léxico de autor-tiempo.

Nota: `eval(..)` cuando se utiliza en un programa en modo estricto opera en su propio ámbito léxico, lo que significa que las declaraciones hechas dentro del `eval()` no modifican el ámbito de inclusión.

```js
function foo(str) {
   "use strict";
   eval( str );
   console.log( a ); // ReferenceError: a is not defined
}

foo( "var a = 2" );
```

Hay otras instalaciones en JavaScript que equivalen a un efecto muy similar al `eval(..`\). `setTimeout(..)` y `setInterval(..)` pueden tomar una cadena para su primer argumento respectivo, cuyo contenido se evalúa como el código de una función generada dinámicamente. Se trata de un comportamiento antiguo, heredado y desactualizado desde hace mucho tiempo. ¡No lo hagas!

El nuevo constructor de función `function(...)` toma de forma similar una cadena de código en su último argumento para convertirse en una función generada dinámicamente \(el primer argumento, si lo hay, son los parámetros nombrados para la nueva función\). Esta sintaxis de función-constructor es ligeramente más segura que `eval(..)`, pero debe evitarse en su código.

Los casos de uso para la generación dinámica de código dentro de su programa son increíblemente raros, ya que las degradaciones del rendimiento casi nunca vale la pena la capacidad.

### `with`

El otro aspecto con el ceño fruncido \(y ahora desaprobado!\) en JavaScript que engaña el ámbito léxico es la palabra clave `with`. Existen múltiples vías válidas que pueden explicarse, pero voy a elegir aquí para explicarlo desde la perspectiva de cómo interactúa y afecta el alcance léxico.

`with` se explica típicamente como un atajo para hacer referencias múltiples de la característica contra un objeto sin repetir la referencia del objeto mismo cada vez.

Por ejemplo:

```js
var obj = {
	a: 1,
	b: 2,
	c: 3
};

// más "tedioso" repetir "obj"
obj.a = 2;
obj.b = 3;
obj.c = 4;

// atajo "mas fácil"
with (obj) {
	a = 3;
	b = 4;
	c = 5;
}
```

Sin embargo, hay mucho más que hacer aquí que sólo un atajo conveniente para el acceso a la propiedad del objeto. Considere:

```js
function foo(obj) {
	with (obj) {
		a = 2;
	}
}

var o1 = {
	a: 3
};

var o2 = {
	b: 3
};

foo( o1 );
console.log( o1.a ); // 2

foo( o2 );
console.log( o2.a ); // undefined
console.log( a ); // 2 -- Oops, leaked global!
```

En este ejemplo de código, se crean dos objetos `o1` y `o2`. Uno tiene una propiedad y el otro no. La función `foo(..)` toma una referencia de objeto `obj` como un argumento, y se llama `with (obj) {..}` en la referencia. Dentro del bloque `with`, hacemos lo que parece ser una referencia léxica normal a una variable `a`, una referencia LHS de hecho \(ver Capítulo 1\), para asignarle el valor de `2`.

Cuando pasamos a `o1`, la asignación `a = 2` encuentra la propiedad `o1.a` y le asigna el valor `2`, como se refleja en la siguiente sentencia `console.log(o1.a)`. Sin embargo, cuando pasamos en `o2`, ya que no tiene una propiedad `a`, no se crea tal propiedad, y `o2.a` permanece indefinido.

Pero luego observamos un efecto secundario peculiar, el hecho de que una variable global `a` fue creada por la asignación `a = 2`. ¿Cómo puede ser esto posible?

La instrucción `with` toma un objeto, uno que tiene cero o más propiedades, **y trata ese objeto como si fuera un ámbito léxico completamente separado**, por lo tanto las propiedades del objeto son tratadas como identificadores lexicalmente definidos en ese "ámbito".

**Nota**: A pesar de que un bloque `with` trata un objeto como un ámbito léxico, una declaración `var` normal dentro de ese con el bloque no será delimitada con el bloque, sino el ámbito de la función que contiene.

Mientras que la función `eval(...)` puede modificar el alcance léxico existente si toma una cadena de código con una o más declaraciones en ella, la instrucción `with` **crea realmente un ámbito léxico completamente nuevo **desde el aire, desde el objeto al que se pasa .

Entendido de esta manera, el "ámbito" declarado por la instrucción `with` cuando pasamos en `o1` era `o1`, y "scope" tenía un "identificador" en el que corresponde a la propiedad `o1.a`. Pero cuando usamos `o2` como el "ámbito", no tenía tal "identificador" en él, por lo tanto las reglas normales de identificación de consulta LHS \(véase el capítulo 1\) se produce.

Ni el "alcance" de `o2`, ni el alcance de `foo(..)`, ni el alcance global incluso, tiene un identificador `a` para ser encontrado, por lo que cuando se ejecuta `a = 2`, resulta en la creación automática-global que se crea \(Ya que estamos en modo no estricto\).

Es un tipo extraño de pensamiento ver `with` cambiar, en tiempo de ejecución, un objeto y sus propiedades en un "ámbito" con "identificadores". Pero esa es la explicación más clara que puedo dar para los resultados que vemos.

**Nota**: Además de ser una mala idea de usar, ambos `eval(..)` y `with` son afectados \(restringido\) por el modo estricto. `With` es descartada completamente, mientras que varias formas de `eval(...)` indirecto o inseguro son rechazadas mientras que retiene la funcionalidad del core.

### Rendimiento

Ambos `eval(..)` y `with` engañan el ámbito léxico definido por autor-tiempo modificando o creando un nuevo ámbito léxico en tiempo de ejecución.

Entonces, ¿cuál es el problema, usted preguntará? Si ofrecen funcionalidad más sofisticada y flexibilidad de codificación, ¿no son estas buenas características? No.

El Motor de JavaScript tiene una serie de optimizaciones de rendimiento que realiza durante la fase de compilación. Algunos de estos se reducen a ser capaz de analizar esencialmente estáticamente el código como lexes, y pre-determinar dónde están todas las declaraciones de variables y funciones, por lo que se necesita menos esfuerzo para resolver los identificadores durante la ejecución.

Pero si el Motor encuentra un `eval(...)` o un `with` en el código, esencialmente tiene que asumir que toda su conciencia de la localización del identificador puede ser inválida, porque no puede saber a la hora exacta en que el código puede pasar a `eval(..)` para modificar el ámbito léxico o el contenido del objeto con el que se puede pasar para crear un nuevo ámbito léxico a consultar.

En otras palabras, en el sentido pesimista, la mayoría de las optimizaciones que haría son inútiles si `eval(..)` o `with` están presentes, por lo que simplemente no realiza las optimizaciones en absoluto.

Su código casi siempre tiende a correr más lento simplemente por el hecho de que incluye un `eval(..)` o un `with` en cualquier parte del código. No importa lo inteligente que sea el Motor para tratar de limitar los efectos secundarios de estos supuestos pesimistas, **no hay manera de evitar que, sin las optimizaciones, el código funcione más lentamente**.

