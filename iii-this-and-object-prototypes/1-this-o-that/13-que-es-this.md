# 1.3 ¿Que es `this`?

Después de apartar varias suposiciones incorrectas, volvamos ahora nuestra atención a cómo funciona realmente el mecanismo `this` .

Hemos dicho anteriormente que `this` no es una vinculación autor-tiempo sino un enlace de tiempo de ejecución. Es contextual basado en las condiciones de la invocación de la función. El alcance `this` no tiene nada que ver con el lugar donde se declara una función, sino que tiene en cambio que ver con la forma en que se llama a la función.

Cuando se invoca una función, se crea un registro de activación, también conocido como contexto de ejecución. Este registro contiene información sobre dónde se llamó la función \(la pila de llamadas\), cómo se invocó la función, qué parámetros se pasaron, etc. Una de las propiedades de este registro es la referencia de `this` que se utilizará durante la duración de la ejecución de esa función.

En el próximo capítulo, aprenderemos a encontrar el **sitio de llamada** de una función para determinar cómo su ejecución vinculará a `this`.



