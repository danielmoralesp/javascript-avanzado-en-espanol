# 1.2 Entendiendo el Scope

La forma en que abordaremos el aprendizaje sobre el scope es pensar en el proceso en términos de una conversación. Pero, ¿quién está teniendo la conversación?

### El elenco

Vamos a conocer el elenco de personajes que interactúan para procesar el programa `var a = 2;`, por lo que entendemos sus conversaciones que escucharemos en breve:

1. Motor: responsable de la compilación y ejecución de principio a fin de nuestro programa JavaScript.
2. Compilador: uno de los amigos del Motor; Maneja todo el trabajo sucio de análisis y generación de código \(ver sección anterior\).
3. Scope: otro amigo del Motor; Recopila y mantiene una lista de búsqueda de todos los identificadores declarados \(variables\), y aplica un conjunto estricto de reglas sobre cómo estos son accesibles para el código que se está ejecutando actualmente.

Para que entiendas completamente cómo funciona JavaScript, debes comenzar a pensar como Motor \(y amigos\), hacer las preguntas que hacen y responder a esas preguntas de la misma manera.

### Atrás y adelante

Cuando vea el programa `var a = 2;`, lo más probable es pensar en eso como una sola declaración. Pero no es así como nuestro nuevo amigo Motor lo ve. De hecho, Motor ve dos declaraciones distintas, una que el compilador manejará durante la compilación, y una que el motor manejará durante la ejecución.

Así que, vamos a desglosar cómo Motor y amigos se acercará al programa `var a = 2`;.

Lo primero que Compilador hará con este programa es realizar lexing para dividirlo en fichas, que luego analizará en un árbol. Pero cuando Compilador llega a la generación de código, tratará este programa de manera algo diferente de lo que se supone.

Una suposición razonable sería que el compilador producirá código que podría ser resumido por este pseudo-código: "Asignar memoria para una variable, etiquetar la `a`, y luego pegar el valor `2` en esa variable". Desafortunadamente, eso no es muy exacto.

El compilador procederá como sigue:

1. Encuentro `var a`, Compilador pregunta a Scope para ver si ya existe una variable `a` para esa colección de ámbito en particular. Si es así, el compilador omite esta declaración y sigue adelante. De lo contrario, el compilador solicita a Scope que declare una nueva variable llamada `a` para esa colección de ámbito.
2. El Compilador entonces produce código para que el motor ejecute más adelante, para manejar la asignación `a = 2`. El código que Motor ejecuta primero preguntará a Scope si hay una variable llamada accesible en la colección de alcance actual. Si es así, el motor utiliza esa variable. Si no, el motor busca en otra parte \(vea la sección de ámbito anidada abajo\).

Si finalmente el Motor encuentra una variable, le asigna el valor `2`. ¡Si no, el motor levantará su mano y gritará hacia fuera un error!

En resumen, se toman dos acciones distintas para una asignación de variables: Primero, el Compilador declara una variable \(si no se ha declarado previamente en el ámbito actual\) y segundo, al ejecutar, Motor busca la variable en Scope y la asigna a ella, si se encuentra.

### Compiler Speak

Necesitamos un poco más de terminología del compilador para continuar con la comprensión.

Cuando el Motor ejecuta el código que el Compilador produjo para el paso \(2\), tiene que buscar la variable `a` para ver si se ha declarado, y esta búsqueda está consultando el Scope. Pero el tipo de Motor de búsqueda que realiza afecta el resultado de la búsqueda.

En nuestro caso, se dice que Motor estaría realizando una búsqueda "LHS" para la variable `a`. El otro tipo de búsqueda se llama "RHS".

Apuesto a que usted puede adivinar lo que la "L" y "R" significa. Estos términos representan "Lado Izquierdo" y "Lado Derecho".

Lado ... ¿de qué? **De una operación de asignación**.

En otras palabras, una búsqueda LHS se hace cuando aparece una variable en el lado izquierdo de una operación de asignación y una búsqueda RHS se hace cuando aparece una variable en el lado derecho de una operación de asignación.

En realidad, seamos un poco más precisos. Una búsqueda de RHS es indistinguible, para nuestros propósitos, de simplemente una mirada hacia arriba del valor de alguna variable, mientras que la búsqueda de LHS está tratando de encontrar el contenedor de la variable en sí, para que pueda asignar. De esta manera, RHS no significa realmente "lado derecho de una asignación" per se, sino que, más exactamente, significa "no a la izquierda".

Siendo ligeramente glib por un momento, también podría pensar "RHS" en su lugar significa "recuperar su fuente \(valor\)", lo que implica que RHS significa "ir a buscar el valor de ...".

Vamos a cavar en eso más profundo.

Cuando yo digo:

```js
console.log( a );
```

La referencia `a` es una referencia RHS, porque nada está siendo asignado aun aquí. En su lugar, estamos buscando para recuperar el valor de `a`, por lo que el valor se puede pasar a `console.log(..)`.

En contraste:

```js
a = 2;
```

La referencia `a` aquí es una referencia LHS, porque realmente no nos importa cuál es el valor actual, simplemente queremos encontrar la variable como un objetivo para la operación de asignación `= 2`.

**Nota**: LHS y RHS que significan "Lado izquierdo / derecho de una asignación" no significa necesariamente literalmente "lado izquierdo / derecho del operador de asignación". Hay varias otras maneras en que ocurren las asignaciones, por lo que es mejor pensar conceptualmente en ella como: "_quién es el objetivo de la asignación \(LHS\) - estoy asignando_" y "_quién es la fuente de la asignación \(RHS\) - estoy buscando_".

Considere este programa, que tiene tanto referencias LHS y RHS :

```js
function foo(a) {
	console.log( a ); // 2
}

foo( 2 );
```

La última línea que invoca `foo(..)` como una llamada de función requiere una referencia RHS a `foo`, que significa "ir a buscar el valor de `foo`, y dármelo". Además, `(..)` significa que el valor de `foo` debe ser ejecutado, por lo que sería en realidad una función!

Hay una asignación sutil pero importante aquí. ¿Lo viste?

Es posible que haya omitido el implícito `a = 2` en este fragmento de código. Sucede cuando el valor `2` se pasa como argumento a la función `foo(...)`, en cuyo caso el valor `2` se asigna al parámetro `a`. Para asignar \(implícitamente\) al parámetro `a`, se realiza una búsqueda LHS.

También hay una referencia RHS para el valor de `a`, y ese valor resultante se pasa a `console.log(..)`. `console.log(..)`necesita una referencia para ejecutarse. Es una búsqueda de RHS para el objeto de `console`, entonces se produce una resolución de propiedad para ver si tiene un método llamado `log`.

Por último, podemos conceptualizar que hay un intercambio LHS / RHS de pasar el valor `2` \(a través de la variable `a` de búsqueda RHS\) en `log(..)`. Dentro de la implementación nativa de `log(..)`, podemos suponer que tiene parámetros, el primero de los cuales \(tal vez llamado `arg1`\) tiene una referencia de referencia LHS, antes de asignar `2` a ella.

**Nota**: Es posible que se sienta tentado a conceptualizar la función de declaración de `function foo(a) {...` como una declaración y asignación de variables normales, como `var foo` y `foo = function(a) {....` Al hacerlo, sería ser tentador pensar que esta declaración de función implica una búsqueda de LHS.

Sin embargo, la diferencia sutil pero importante es que el Compilador maneja la declaración y la definición de valor durante la generación de código, de modo que cuando el Motor está ejecutando el código, no hay ningún procesamiento necesario para "asignar" un valor de función a `foo`. Por lo tanto, no es realmente apropiado pensar en una declaración de función como una asignación de búsqueda LHS en la forma en que estamos discutiendo aquí.

### Conversación entre el Motor y el Scope

```js
function foo(a) {
	console.log( a ); // 2
}

foo( 2 );
```

Imaginemos el intercambio anterior \(que procesa este fragmento de código\) como una conversación. La conversación sería algo como esto:

> Motor: Hey Scope, tengo una referencia RHS para `foo`. ¿Has oído hablar de él?
>
> Scope: Por qué? sí, lo he hecho. El Compilador lo declaró hace apenas un segundo. Es una función. Aqui tienes.
>
> Motor: Genial, gracias! OK, estoy ejecutando `foo`.
>
> Motor: Hey, Scope, tengo una referencia de LHS para `a`, alguna vez oído hablar de él?
>
> Scope: Por qué sí?, lo he hecho. El Compilador lo declaró como un parámetro formal para `foo` hace poco. Aqui tienes.
>
> Motor: Útil como siempre, Scope. Gracias de nuevo. Ahora, es tiempo para asignar `2` a `a`.
>
> Motor: Oye, Scope, siento molestarte de nuevo. Necesito una búsqueda de RHS para la `console`. ¿Has oído hablar de él?
>
> Scope: No hay problema, Motor, esto es lo que hago todo el día. Sí, tengo `console`. Está integrado. Aqui tienes.
>
> Motor: Perfecto. Buscando el  `log(..)`. OK, genial, es una función.
>
> Motor: Yo, Scope. ¿Puedes ayudarme con una referencia de RHS a `a`. Creo que lo recuerdo, pero sólo quiero comprobarlo.
>
> Scope: Usted tiene razón, Motor. El mismo tipo, no ha cambiado. Aqui tienes.
>
> Motor: Cool. Pasando el valor de `a`, que es `2`, en `log(..)`.
>
> ...

### Examen

Compruebe su comprensión hasta ahora. Asegúrese de jugar con la parte de Motor y tener una "conversación" con el Scope:

```js
function foo(a) {
	var b = a;
	return a + b;
}

var c = foo( 2 );
```

1. Identifique todas las consultas LHS \(hay 3!\).
2. Identifique todas las consultas RHS \(hay 4!\).

**Nota**: ¡Consulte la revisión del capítulo para las respuestas del examen!







