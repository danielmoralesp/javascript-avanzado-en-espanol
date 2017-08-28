# 3.3 Funciones como ámbitos

Hemos visto que podemos tomar cualquier fragmento de código y envolver una función alrededor de él, y que efectivamente "oculta" cualquier declaración de función o variable encerrada desde el ámbito externo dentro del ámbito interno de esa función.

Por ejemplo:

```js
var a = 2;

function foo() { // <-- insert this

    var a = 3;
    console.log( a ); // 3

} // <-- and this
foo(); // <-- and this

console.log( a ); // 2
```

Si bien esta técnica "funciona", no es necesariamente ideal. Hay algunos problemas que introduce. El primero es que tenemos que declarar una función nombrada `foo()`, lo que significa que el nombre del identificador `foo` mismo "contamina" el ámbito de inclusión \(global, en este caso\). También tenemos que llamar explícitamente a la función por nombre \(`foo()`\) para que el código envuelto en realidad se ejecute.

Sería más ideal si la función no necesitaba un nombre \(o, más bien, el nombre no contamine el ámbito de inclusión\) y si la función podría ejecutarse automáticamente.

Afortunadamente, JavaScript ofrece una solución a ambos problemas.

