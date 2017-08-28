# 4.1 ¿El Huevo o la Gallina?

Hay una tentación de pensar que todo el código que se ve en un programa JavaScript se interpreta línea por línea, de arriba hacia abajo en orden, tal y como se ejecuta el programa. Si bien eso es sustancialmente cierto, hay una parte de esa suposición que puede conducir a un pensamiento incorrecto sobre su programa.

Considere este código:

```js
a = 2;

var a;

console.log( a );
```

¿Qué espera imprimir en la sentencia `console.log(..)`?

Muchos desarrolladores esperan `undefined`, ya que la instrucción `var a` viene después de `a = 2`, y parece natural asumir que la variable es re-definida, y por lo tanto asignado el `undefined`. Sin embargo, la salida será `2`.

Considere otra pieza de código:

```js
console.log( a );

var a = 2;
```

Es posible que se sienta tentado a asumir que, dado que el fragmento anterior mostraba un comportamiento de apariencia inferior a superior, tal vez en este fragmento, también se imprimirá `2`. Otros pueden pensar que ya que la variable `a` se utiliza antes de que se declara, esto debe resultar en un `ReferenceError`.

Desafortunadamente, ambas conjeturas son incorrectas. `undefined` es la salida.

Entonces, ¿qué está pasando aquí? Parece que tenemos una pregunta sobre tipo gallina o huevo. ¿Cuál viene primero, la declaración \("huevo"\), o la asignación \("gallina"\)?

