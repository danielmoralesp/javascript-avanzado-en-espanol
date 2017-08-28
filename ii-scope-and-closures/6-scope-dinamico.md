# 6- Scope Dinámico

En el capítulo 2, hablamos de "Ámbito dinámico" como un contraste con el modelo "Ámbito Léxico", que es cómo funciona el ámbito en JavaScript \(y de hecho, la mayoría de los otros lenguajes\).

Vamos a examinar brevemente el alcance dinámico, para martillar sobre las diferencias. Pero, lo que es más importante, el alcance dinámico es realmente un primo cercano a otro mecanismo \(`this`\) en JavaScript, que cubrimos en el título "This y los Prototipos de Objetos" de esta serie de libros.

Como vimos en el capítulo 2, el ámbito léxico es el conjunto de reglas sobre cómo el motor puede buscar una variable y dónde la encontrará. La característica clave del alcance léxico es que se define a tiempo de autor, cuando se escribe el código \(suponiendo que no lo engañe con `eval()`o `with`\).

El alcance dinámico parece implicar, y por buena razón, que hay un modelo por el cual el alcance se puede determinar dinámicamente en tiempo de ejecución, en vez de estaticamente a tiempo de autor. Ése es de hecho el caso. Vamos a ilustrarlo a través del código:

```js
function foo() {
    console.log( a ); // 2
}

function bar() {
    var a = 3;
    foo();
}

var a = 2;

bar();
```

El ámbito léxico sostiene que la referencia RHS a un in `foo()` será resuelta a la variable global `a`, lo que dará como resultado el valor `2`.

Por el contrario, **el ámbito dinámico no se ocupa de cómo y dónde se declaran las funciones y los ámbitos, sino de dónde se llaman**. En otras palabras, la cadena de alcance se basa en la pila de llamadas \(call-stack\), no en la anidación de ámbitos en el código.

Por lo tanto, si JavaScript tiene un ámbito dinámico, cuando se ejecuta `foo()`, teóricamente el código siguiente resultará en `3` como salida.

```js
function foo() {
    console.log( a ); // 3  (not 2!)
}

function bar() {
    var a = 3;
    foo();
}

var a = 2;

bar();
```

¿Cómo puede ser posible esto? Debido a que cuando `foo()` no puede resolver la referencia de variable para `a`, en lugar de intensificar la cadena de alcance anidada \(lexical\), va hasta la pila de llamadas, para encontrar dónde se llamó `foo()`. Desde que `foo()` fue llamado desde `bar()`, comprueba las variables en el ámbito de `bar()`, y encuentra un `a` con valor `3`.

¿Extraño? Probablemente pienses así, por el momento.

Pero eso es sólo porque probablemente sólo has trabajado en \(o al menos profundamente considerado\) código que tiene un alcance léxico. Por lo tanto, el alcance dinámico parece extraño. Si sólo hubieras escrito código en un lenguaje con un alcance dinámico, parecería natural, y el ámbito léxico sería la cosa extraña.

Para ser claro, J**avaScript no tiene, de hecho, un ámbito dinámico**. Tiene alcance léxico. Llano y simple. Pero el mecanismo de este tipo es como un ámbito dinámico.

El contraste clave: **el ámbito léxico es write-time, mientras que el ámbito dinámico \(y **`this`**!\) son runtime**. El alcance lexical cuida donde una función _fue declarada_, pero los concerns dinámicas del alcance de donde una función _fue llamada_.

Por último: `this` le importa cómo se llamó una función, lo que demuestra lo estrechamente relacionado que este mecanismo esta con la idea del alcance dinámico. Para profundizar más en `this`, lea el título "this & Object Prototipos".

