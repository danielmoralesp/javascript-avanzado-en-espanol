# 1.4 Valores y Tipos

Si le preguntas a un empleado en una tienda de teléfono cuánto cuesta un teléfono determinado, y dicen "noventa y nueve, noventa y nueve" \(es decir, $ 99.99\), te están dando una cifra numérica real que representa lo que vas a necesitar pagar \(más impuestos\) para comprarlo. Si usted quiere comprar dos de esos teléfonos, puede hacer fácilmente las matemáticas mentales para duplicar ese valor para obtener $ 199.98 por su costo base.

Si ese mismo empleado coge otro teléfono similar, pero dice que es "gratis", no le están dando un número, sino otro tipo de representación de su costo esperado \($ 0.00\) - la palabra "free ."

Cuando más tarde pregunte si el teléfono incluye un cargador, esa respuesta sólo podría haber sido "sí" o "no".

De manera muy similar, cuando expresa valores en un programa, elige diferentes representaciones para esos valores en función de lo que planea hacer con ellos.

Estas diferentes representaciones de los valores se denominan tipos en terminología de programación. JavaScript tiene tipos incorporados para cada uno de estos llamados valores primitivos:

* Cuando necesitas hacer matemáticas, quieres un número.
* Cuando necesite imprimir un valor en la pantalla, necesita una cadena \(uno o más caracteres, palabras, oraciones\).
* Cuando necesite tomar una decisión en su programa, necesita un booleano \(verdadero o falso\).

Los valores que se incluyen directamente en el código fuente se llaman literales. Los literales de cadenas están rodeados por comillas dobles "..." o comillas simples \('...'\) - la única diferencia es la preferencia estilística. El número y los literales booleanos se presentan como es \(es decir, 42, true, etc.\).

Considere:

```js
"I am a string";
'I am also a string';

42;

true;
false;
```

Más allá de los tipos de valores string / number / boolean, es común que los lenguajes de programación proporcionen arrays, objetos, funciones y más. Cubriremos mucho más sobre valores y tipos a lo largo de este capítulo y el siguiente.

### Conversión entre tipos

Si tienes un número pero necesitas imprimirlo en la pantalla, necesitas convertir el valor en una cadena, y en JavaScript esta conversión se llama "coerción". Del mismo modo, si alguien introduce una serie de caracteres numéricos en un formulario en una página de comercio electrónico, es una cadena, pero si necesita utilizar ese valor para realizar operaciones matemáticas, debe coaccionarla a un número.

JavaScript proporciona varias facilidades diferentes para forzar la coerción entre tipos. Por ejemplo:

```js
var a = "42";
var b = Number( a );

console.log( a );	// "42"
console.log( b );	// 42
```

El uso de Number \(..\) \(una función incorporada\) como se muestra es una coerción explícita de cualquier otro tipo al tipo de número. Eso debería ser bastante sencillo.

Pero un tema polémico es lo que sucede cuando se intenta comparar dos valores que ya no son del mismo tipo, lo que requeriría coerción implícita.

Al comparar la cadena "99.99" con el número 99.99, la mayoría de la gente estaría de acuerdo en que son equivalentes. Pero no son exactamente iguales, ¿verdad? Es el mismo valor en dos representaciones diferentes, dos tipos diferentes. Se podría decir que son "vagamente iguales", ¿no?

Para ayudarte en estas situaciones comunes, JavaScript a veces se activa y obliga implícitamente a los valores a los tipos coincidentes.

Así que si usas el operador == loose igual para hacer la comparación "99.99" == 99.99, JavaScript convertirá el lado izquierdo "99.99" a su número equivalente 99.99. La comparación entonces se convierte en 99.99 == 99.99, que es por supuesto verdad.

Si bien está diseñado para ayudarlo, la coerción implícita puede crear confusión si no se ha tomado el tiempo para aprender las reglas que rigen su comportamiento. La mayoría de los desarrolladores de JS nunca lo tienen, por lo que la sensación común es que la coerción implícita es confusa y daña los programas con errores inesperados, por lo que debe evitarse. Incluso a veces se llama un defecto en el diseño del lenguaje.

Sin embargo, la coerción implícita es un mecanismo que se puede aprender, y además debe ser aprendido por cualquiera que desee tomar la programación de JavaScript en serio. ¡No sólo no confunde una vez que aprende las reglas, él puede realmente hacer sus programas mejores! El esfuerzo vale la pena.

Nota: Para obtener más información sobre la coerción, consulte el Capítulo 2 de este título y el Capítulo 4 del título de Tipos y Gramática de esta serie.

