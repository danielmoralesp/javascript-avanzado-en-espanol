# 1.2 Inténtalo tú mismo

Este capítulo va a introducir cada concepto de programación con simples fragmentos de código, todos escritos en JavaScript \(obviamente!\).

No se puede enfatizar lo suficiente: mientras que usted va a través de este capítulo - y usted necesitará darle el tiempo para repasarlo varias veces - usted debe practicar cada uno de estos conceptos escribiendo el código usted mismo. La forma más sencilla de hacerlo es abrir la consola de herramientas de desarrollador en el navegador más cercano \(Firefox, Chrome, IE, etc.\).

Sugerencia: Normalmente, puede iniciar la consola del programador con un acceso directo de teclado o desde un elemento de menú. Para obtener información más detallada acerca del inicio y el uso de la consola en su navegador favorito, consulte "Dominar de la consola de herramientas de desarrollo" \([http://blog.teamtreehouse.com/mastering-developer-tools-console](http://blog.teamtreehouse.com/mastering-developer-tools-console)\). Para escribir varias líneas en la consola a la vez, use &lt;shift&gt; + &lt;enter&gt; para pasar a la nueva línea siguiente. Una vez que pulse &lt;enter&gt; por sí mismo, la consola ejecutará todo lo que acaba de escribir.

Vamos a familiarizarnos con el proceso de ejecución de código en la consola. En primer lugar, sugiero abrir una pestaña vacía en su navegador. Prefiero hacer esto escribiendo: ​​en blanco en la barra de direcciones. A continuación, asegúrese de que su consola de desarrollo está abierta, como acabamos de mencionar.

Ahora, escriba este código y vea cómo se ejecuta:

```js
a = 21;

b = a * 2;

console.log( b );
```

Escribir el código anterior en la consola en Chrome debería producir algo como lo siguiente:

![](/assets/Captura de pantalla 2017-08-24 a las 2.18.31 p.m..png)

Vamos, prueba. La mejor manera de aprender la programación es empezar a codificar!

### Salida

En el fragmento de código anterior, utilizamos console.log \(..\). En pocas palabras, veamos de qué se trata esa línea de código.

Usted puede haber adivinado, pero eso es exactamente cómo imprimimos el texto \(aka salida al usuario\) en la consola del navegador. Hay dos características de esa afirmación que debemos explicar.

En primer lugar, la parte log \(b\) se denomina llamada de función \(véase "Funciones"\). Lo que pasa es que estamos entregando la variable b a esa función, que le pide tomar el valor de b e imprimirlo en la consola.

En segundo lugar, la parte console es una referencia de objeto donde se encuentra la función log \(..\). Cubrimos los objetos y sus propiedades con más detalle en el Capítulo 2.

Otra forma de crear resultados que puede ver es ejecutar una instrucción de alert\(..\). Por ejemplo:

```js
alert( b );
```

Si ejecuta eso, notará que en lugar de imprimir la salida a la consola, muestra un cuadro emergente "Aceptar" con el contenido de la variable b. Sin embargo, usar console.log \(...\) generalmente hará que el aprendizaje sobre la codificación y ejecución de sus programas en la consola sea más fácil que usar la alert \(..\), ya que puede generar muchos valores a la vez sin interrumpir la interfaz del navegador.

Para este libro, usaremos console.log \(..\) para la salida.

### Entrada

Mientras estamos discutiendo la salida, también puede preguntarse sobre la entrada \(es decir, recibir información del usuario\).

La forma más común que ocurre es que la página HTML muestre elementos de formulario \(como cuadros de texto\) en un usuario al que puedan escribir y, a continuación, utilizar JS para leer esos valores en las variables del programa.

Pero hay una manera más fácil de obtener información para fines sencillos de aprendizaje y demostración, como la que estará haciendo a lo largo de este libro. Utilice la función de prompt \(..\):

```js
age = prompt( "Please tell me your age:" );

console.log( age );
```

Como ya habrás adivinado, el mensaje que pasas al prompt \(..\) - en este caso, "Por favor, dime tu edad:" - se imprime en el popup.

Esto debería ser similar a lo siguiente:

![](/assets/Captura de pantalla 2017-08-24 a las 2.24.25 p.m..png)

Una vez que envíe el texto de entrada haciendo clic en "Aceptar", observará que el valor que escribió se almacena en la variable de edad, que luego se emite con console.log \(..\):

![](/assets/Captura de pantalla 2017-08-24 a las 2.25.11 p.m..png)

Para mantener las cosas simples mientras estamos aprendiendo conceptos básicos de programación, los ejemplos en este libro no requerirán entrada. Pero ahora que has visto cómo usar el prompt \(..\), si quieres desafiarte puedes intentar usar la entrada en tus exploraciones de los ejemplos.







