# 3.5 Revisión \(TL; DR\)

Las funciones son la unidad de scope más común en JavaScript. Las variables y las funciones que se declaran dentro de otra función están esencialmente "ocultas" de cualquiera de los "ámbitos" incluidos, que es un principio de diseño intencional de un buen software.

Pero las funciones no son de ninguna manera la única unidad de ámbito. Block-scope se refiere a la idea de que las variables y las funciones pueden pertenecer a un bloque arbitrario \(generalmente, cualquier par `{..} `\) de código, en lugar de sólo a la función de inclusión.

Comenzando con ES3, la estructura `try/catch` tiene block-scope en la cláusula `catch`.

En ES6, se introduce la palabra clave `let` \(un primo de la palabra clave `var`\) para permitir declaraciones de variables en cualquier bloque arbitrario de código. `if(..) {let a = 2; }` Declarará una variable `a` que esencialmente perteneciente a el alcance del bloque `{..}` de `if` y se adjunta allí.

Aunque algunos parecen creerlo así, el alcance del bloque no debe ser tomado como un reemplazo directo del alcance de la función `var`. Ambas funcionalidades coexisten, y los desarrolladores pueden y deben utilizar tanto las técnicas de alcance de función como de ámbito de bloque, donde son apropiadas para producir un código mejor, más legible y más fácil de mantener.

