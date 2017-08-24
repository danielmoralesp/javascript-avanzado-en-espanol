# 1.5 Comentarios del Código

El empleado de la tienda telefónica puede anotar algunas notas sobre las características de un teléfono recién lanzado o sobre los nuevos planes que su empresa ofrece. Estas notas son sólo para el empleado - no son para los clientes leer. Sin embargo, estas notas ayudan al empleado a hacer su trabajo mejor documentando de los comos y los porqués de lo que ella debe decir a los clientes.

Una de las lecciones más importantes que puede aprender sobre la escritura de código es que no es sólo para la computadora. El código es mucho más, si no más, para el desarrollador tanto como lo es para el compilador.

Su computadora sólo se preocupa por el código de máquina, una serie de 0s y 1s binarios, que viene de la compilación. Hay un número casi infinito de programas que podría escribir que producen la misma serie de 0s y 1s. Las opciones que hagas sobre cómo escribir tu programa son importantes, no sólo para ti, sino para los demás miembros del equipo e incluso para tu futuro yo.

Usted debe esforzarse no sólo en escribir programas que funcionen correctamente, sino programas que tienen sentido cuando se examinan. Puede recorrer un largo camino en ese esfuerzo eligiendo buenos nombres para sus variables \(ver "Variables"\) y funciones \(ver "Funciones"\).

Pero otra parte importante es los comentarios del código. Éstos son pedacitos de texto en su programa que se insertan puramente para explicar cosas a un ser humano. El intérprete / compilador siempre ignorará estos comentarios.

Hay un montón de opiniones sobre lo que debe ser un buen código comentado; No podemos definir reglas universales absolutas. Pero algunas observaciones y directrices son muy útiles:

* El código sin comentarios es subóptimo.
* Demasiados comentarios \(uno por línea, por ejemplo\) es probablemente un signo de código mal escrito.
* Los comentarios deben explicar por qué, no qué hacen. Opcionalmente pueden explicar cómo si eso es particularmente confuso.

En JavaScript, hay dos tipos de comentarios posibles: un comentario de una sola línea y un comentario de varias líneas.

Considere:

```js
// This is a single-line comment

/* But this is
       a multiline
             comment.
                      */
```

El // comentario de una sola línea es apropiado si vas a poner un comentario justo encima de una sola sentencia, o incluso al final de una línea. Todo en la línea después de // se trata como el comentario \(por lo tanto es ignorado por el compilador\), todo el camino hasta el final de la línea. No hay ninguna restricción a lo que puede aparecer dentro de un comentario de una sola línea.

Considere:

```js
var a = 42;		// 42 is the meaning of life
```

El comentario de / \* .. \* / multiline es apropiado si tienes varias líneas de explicación para hacer en tu comentario.

Aquí está un uso común de comentarios multilínea:

```js
/* The following value is used because
   it has been shown that it answers
   every question in the universe. */
var a = 42;
```

También puede aparecer en cualquier parte de una línea, incluso en el centro de una línea, porque el \* / lo termina. Por ejemplo:

```js
var a = /* arbitrary value */ 42;

console.log( a );	// 42
```

Lo único que no puede aparecer dentro de un comentario de varias líneas es un \* /, porque eso sería interpretado para finalizar el comentario.

Usted definitivamente querrá comenzar su aprendizaje de programación comenzando con el hábito de comentar el código. A lo largo del resto de este capítulo, verás que utilizo los comentarios para explicar las cosas, así que haz lo mismo en tu propia práctica. Confía en mí, todo el mundo que lea su código se lo agradecerá!



