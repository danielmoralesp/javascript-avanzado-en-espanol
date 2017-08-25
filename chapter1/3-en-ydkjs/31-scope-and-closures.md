# 3.1 Scope & Closures

Quizás una de las cosas más fundamentales que necesitará para llegar a un entendimiento rápido es cómo el scope de las variables realmente funcionan en JavaScript. No basta con tener creencias anecdóticas difusas sobre el scope.

El título Scope & Closures comienza por desacreditar la idea errónea común de que JS es un "lenguaje interpretado" y por lo tanto no se compila. Nope.

El motor JS compila su código justo antes \(y a veces durante!\) la ejecución. Así que buscamos una comprensión más profunda de la aproximación del compilador a nuestro código para entender cómo encuentra y trata las declaraciones de variables y funciones. A lo largo del camino, vemos la metáfora típica para la gestión del scope de variables en JS, "Hoisting".

Esta comprensión crítica del "alcance léxico" es en lo que entonces basamos nuestra exploración de los closures para el último capítulo del libro. Los closures son tal vez el concepto más importante en todos los de JS, pero si no ha comprendido con firmeza cómo funciona el scope, el closure probablemente permanecerá fuera de su alcance.

Una aplicación importante de closures es el patrón de module, como brevemente se introdujo en este libro en el capítulo 2. El patrón de módulo es quizás el patrón de organización de código más prevalente en todo el código JavaScript; La comprensión profunda de ella debe ser una de sus prioridades más altas.



