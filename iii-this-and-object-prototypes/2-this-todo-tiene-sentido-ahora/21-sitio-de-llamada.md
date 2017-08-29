# 2.1 Sitio de llamada

Para entender la vinculación `this`, tenemos que entender el call-site: la ubicación en el código donde se llama una función \(**no donde se declara**\). Tenemos que inspeccionar el sitio de llamada para responder a la pregunta: ¿es `this` una referencia?

Encontrar el sitio de llamada se trata generalmente: "de ir a localizar desde donde se llama una función", pero no siempre es tan fácil, ya que ciertos patrones de codificación pueden oscurecer el verdadero sitio de llamada.

Lo importante es pensar en la pila de llamadas \(la pila de funciones que se han llamado para llegar al momento actual en ejecución\). El sitio de llamada que nos interesa es en la invocación antes de la función en ejecución.

Vamos a demostrar call-stack y call-site:

```js
function baz() {
    // call-stack es: `baz`
    // así, nuestro sitio de llamada está en el ámbito global

    console.log( "baz" );
    bar(); // <-- call-site para `bar`
}

function bar() {
    // call-stack es: `baz` -> `bar`
    // así, nuestro sitio de llamada está en `baz`

    console.log( "bar" );
    foo(); // <-- call-site para `foo`
}

function foo() {
    // call-stack es: `baz` -> `bar` -> `foo`
    // asi, nuestro sitio de llamada esta en `bar`

    console.log( "foo" );
}

baz(); // <-- call-site para `baz`
```

Tenga cuidado al analizar el código para encontrar el sitio de llamada real \(desde la pila de llamadas\), porque es lo único que importa para el enlace de `this`.

**Nota**: Puede visualizar una pila de llamadas en su mente mirando la cadena de llamadas de función en orden, como hicimos con los comentarios en el fragmento anterior. Pero esto es laborioso y propenso a errores. Otra forma de ver la pila de llamadas es usar una herramienta de depuración en su navegador. La mayoría de los navegadores de escritorio modernos incorporan herramientas de desarrollo, que incluyen un depurador JS. En el fragmento anterior, podría haber establecido un punto de interrupción en las herramientas para la primera línea de la función `foo()` o simplemente insertado el `debugger`; en la declaración de esa primera línea. Al ejecutar la página, el depurador se detendrá en esta ubicación y le mostrará una lista de las funciones que se han llamado para llegar a esa línea, que será la pila de llamadas. Por lo tanto, si está intentando diagnosticar la vinculación de `this`, utilice las herramientas de desarrollador para obtener la pila de llamadas y, a continuación, busque el segundo elemento desde la parte superior y se mostrará el sitio de llamada real.

