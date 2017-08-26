# 1.4 Errores

¿Por qué importa si lo llamamos LHS o RHS?

Debido a que estos dos tipos de consultas se comportan de manera diferente en las circunstancias en las que la variable aún no ha sido declarada \(no se encuentra en ningún ámbito consultado\).

Considere:

```js
function foo(a) {
    console.log( a + b );
    b = a;
}

foo( 2 );
```

Cuando la búsqueda RHS ocurre para `b` la primera vez, no se encuentra. Se dice que esto es una variable "no declarada", porque no se encuentra en el ámbito.

Si una búsqueda RHS falla al encontrar una variable, en cualquier lugar de los Scopes anidados, esto resulta en un `ReferenceError` que está siendo lanzado por el Motor. Es importante tener en cuenta que el error es del tipo `ReferenceError`.

Por el contrario, si el Motor está realizando una búsqueda LHS y llega a la planta superior \(ámbito global\) sin encontrarlo, y si el programa no se está ejecutando en "modo estricto" \[^note-strictmode\], entonces el ámbito global creará una nueva variable de ese nombre en el ámbito global y la devolverá al Motor.

> "No, no había una antes, pero fui útil y creé una para ti".

El "Modo estricto" \[^note-strictmode\], que se agregó en ES5, tiene una serie de comportamientos diferentes del modo normal/relajado/perezoso. Una de estas conductas es que no permite la creación de la variable global automática/implícita. En ese caso, no habría ninguna variable Scope'd global a devolver de una búsqueda LHS, y el Motor lanzaría un `ReferenceError` similarmente al caso RHS.

Ahora, si se encuentra una variable para una búsqueda RHS, pero se intenta hacer algo con su valor que es imposible, como intentar ejecutar como función un valor sin función o hacer referencia a una propiedad en un valor `null` o u`ndefined` , entonces el Motor arroja un tipo diferente de error, llamado `TypeError`.

`ReferenceError` es Scope resolution-failure related, mientras que `TypeError` implica que la resolución Scope tuvo éxito, pero que se ha intentado una acción ilegal/imposible contra el resultado.

