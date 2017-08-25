# 2.9 Non-JavaScript

Hasta ahora, las únicas cosas que hemos cubierto están en el propio lenguaje JS. La realidad es que la mayoría de JS está escrito para funcionar e interactuar con entornos como los navegadores. Un buen pedazo de las cosas que usted escribe en su código es, estrictamente hablando, no controlado directamente por JavaScript. Eso probablemente suena un poco extraño.

El JavaScript más común que no se encuentra en JavaScript es la `API DOM`. Por ejemplo:

```js
var el = document.getElementById( "foo" );
```

La variable de `document` existe como variable global cuando el código se ejecuta en un explorador. No es proporcionado por el motor JS, ni es particularmente controlado por la especificación de JavaScript. Toma la forma de algo que se parece mucho a un objeto JS normal, pero no es exactamente eso. Es un objeto especial, a menudo llamado "host object".

Por otra parte, el método `getElementById(..)` en el documento se parece a una función JS normal, pero es sólo una interfaz expuesta a un método incorporado proporcionado por el DOM de su navegador. En algunos navegadores \(de nueva generación\), esta capa también puede estar en JS, pero tradicionalmente el DOM y su comportamiento se implementa en algo más parecido a C / C ++.

Otro ejemplo es con entrada/salida \(Input / Optput - I/O\).

El favorito de todo el mundo `alert(...)` hace que aparezca un cuadro de mensaje en la ventana del navegador del usuario. `alert(...)` se proporciona a su programa JS por el navegador, no por el propio motor JS. La llamada que realiza envía el mensaje a los elementos internos del navegador y se encarga de dibujar y mostrar el cuadro de mensaje.

Lo mismo ocurre con `console.log(..)`; Su navegador proporciona estos mecanismos y los vincula a las herramientas de desarrollo.

Este libro, y toda esta serie, se centra en el lenguaje JavaScript. Es por eso que no ve ninguna cobertura sustancial de estos mecanismos JavaScript non-JavaScript. Sin embargo, usted necesita ser consciente de ellos, como será en cada programa JS que escriba!

