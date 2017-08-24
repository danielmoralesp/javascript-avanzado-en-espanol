# 1.3 Operadores

Los operadores son la forma en que realizamos acciones sobre variables y valores. Ya hemos visto dos operadores de JavaScript, el `=` y el `*`.

El operador `*` realiza la multiplicación matemática. Lo suficientemente simple, ¿no?

El operador `=` igual se utiliza para la asignación - primero calculamos el valor en el lado derecho \(valor fuente\) de la = y luego ponerlo en la variable que especificamos en el lado izquierdo \(variable objetivo\).

Advertencia: Esto puede parecer un orden inverso extraño para especificar asignación. En lugar de a = 42, algunos podrían preferir voltear el orden para que el valor de origen esté a la izquierda y la variable de destino a la derecha, como 42 -&gt; a \(JavaScript no válido\). Desafortunadamente, la forma ordenada a = 42, y variaciones similares, es bastante frecuente en los lenguajes de programación modernos. Si se siente antinatural, sólo pase algún tiempo ensayando y el orden en su mente lo acostumbrará a ella.

Considere:

```js
a = 2;
b = a + 1;
```

Aquí, asignamos el valor 2 a la variable a. A continuación, obtenemos el valor de la variable a \(todavía 2\), agregamos 1 a ella resultando en el valor 3, luego almacenamos ese valor en la variable b.

Si bien técnicamente no es un operador, necesitará la palabra clave `var` en cada programa, ya que es la forma principal de declarar \(aka crear\) variables \(consulte "Variables"\).

Siempre debe declarar la variable por nombre antes de usarla. Pero sólo es necesario declarar una variable una vez para cada ámbito \(ver "Alcance"\); Se puede utilizar tantas veces como sea necesario. Por ejemplo:

```js
var a = 20;

a = a + 1;
a = a * 2;

console.log( a );	// 42
```

Éstos son algunos de los operadores más comunes en JavaScript:

* Asignación: = como en a = 2.
* Matemáticas: + \(adición\), - \(sustracción\), \* \(multiplicación\) y / \(división\), como en a \* 3.
* Asignación Compuesta : + =, - =, \* =, y / = son operadores compuestos que combinan una operación matemática con asignación, como en a + = 2 \(igual que a = a + 2\).
* Incremento / Decremento: ++ \(incremento\), - \(decremento\), como en a ++ \(similar a a = a + 1\).
* Acceso a la propiedad del objeto:. Como en console.log \(\). Los objetos son valores que contienen otros valores en determinadas ubicaciones denominadas propiedades. `obj.a` significa un valor de objeto llamado obj con una propiedad del nombre a. Las propiedades se pueden acceder alternativamente como obj \["a"\]. Consulte el Capítulo 2.
* Igualdad: == \(suelto-igual\), === \(estricto-igual\),! = \(Suelto no-igual\),! == \(estricto no igual\), como en a == b.
* Consulte "Valores y Tipos" y el Capítulo 2.
* Comparación: &lt;\(menor que\),&gt; \(mayor que\), &lt;= \(menor que o suelto-igual\),&gt; = \(mayor que o suelto-igual\), como en a &lt;= b.
* Consulte "Valores y Tipos" y el Capítulo 2.
* Lógico: && \(y\), \|\| \(O\), como en a \|\| b que selecciona a o b. Estos operadores se usan para expresar condicionales compuestos \(ver "Condicionales"\), como si a o b es verdadero.

Nota: Para obtener más detalles y cobertura de los operadores que no se mencionan aquí, consulte las "Expresiones y operadores" de Mozilla Developer Network \(MDN\) [\(https://developer.mozilla.org/en-US/docs/Web/JavaScript / Guía / Expresiones\_y\_Operadores\).](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Expressions_and_Operators)



