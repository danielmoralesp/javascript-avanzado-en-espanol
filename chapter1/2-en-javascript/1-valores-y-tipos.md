# 2.1 Valores y Tipos

Como hemos afirmado en el Capítulo 1, JavaScript ha escrito valores, no las variables escritas. Están disponibles los siguientes tipos \(de datos\) incorporados:

* `string`
* `number`
* `boolean`
* `null` y `undefined`
* `object`
* `symbol` \(nuevo en ES6\)

JavaScript proporciona un tipo de operador que puede examinar un valor y decirle qué tipo es:

```js
var a;
typeof a;                // "undefined"

a = "hello world";
typeof a;                // "string"

a = 42;
typeof a;                // "number"

a = true;
typeof a;                // "boolean"

a = null;
typeof a;                // "object" -- weird, bug

a = undefined;
typeof a;                // "undefined"

a = { b: "c" };
typeof a;                // "object"
```

El valor de retorno del operador `typeof` es siempre uno de los seis valores \(siete de ES6! - el "tipo `symbol`"\) en `string`. Es decir, `typeof "abc"` devuelve `"string"`, no `string`.

Observe cómo en este fragmento la variable `a` contiene cada tipo diferente de valor, y que a pesar de las apariencias, `typeof a` no está pidiendo el "tipo de `a`", sino más bien para el "tipo del valor actualmente en `a`". Sólo los valores tienen tipos en JavaScript; Las variables son solo contenedores simples para esos valores.

`typeof null` es un caso interesante, porque erróneamente devuelve `"object"`, cuando se espera que devuelva `"null"`.

Advertencia: Este es un error de larga data en JS, pero uno que es probable que nunca va a ser arreglado. Demasiado código en la Web tiene este error y por lo tanto, arreglarlo causaría mucho más errores!

También note `a = undefined`. Definimos explícitamente un valor `undefined`, pero eso no es diferente de una variable que no tiene ningún valor definido, como con la variable `a`; como la línea de la parte superior del fragmento. Una variable puede llegar a este estado de valor "indefinido" de varias maneras diferentes, incluyendo funciones que no devuelven valores y uso del operador `void`.

### Objetos

El tipo `object` se refiere a un valor compuesto en el que se pueden establecer propiedades \(ubicaciones con nombre\) que cada una tiene sus propios valores de cualquier tipo. Este es quizás uno de los tipos de valor más útiles en todo JavaScript.

```js
var obj = {
    a: "hello world",
    b: 42,
    c: true
};

obj.a;        // "hello world"
obj.b;        // 42
obj.c;        // true

obj["a"];    // "hello world"
obj["b"];    // 42
obj["c"];    // true
```

Puede ser útil pensar visualmente en este valor `obj`:

![](/assets/Captura de pantalla 2017-08-24 a las 9.41.40 p.m..png)

Se puede acceder a las propiedades con notación de puntos \(es decir, `obj.a`\) o con notación de paréntesis \(es decir, `obj["a"]`\). La notación de puntos es más corta y por lo general más fácil de leer, y por lo tanto se prefiere cuando es posible.

La notación de paréntesis es útil si tienes un nombre de propiedad que tiene caracteres especiales en él, como `obj["¡hola mundo!"]` - tales propiedades se refieren a menudo como llaves cuando se accede a través de la notación de paréntesis. La notación `[]` requiere una variable \(explicada a continuación\) o un `string` literal \(que debe ser envuelto en `".."` o `'..'`\).

Por supuesto, la notación de paréntesis también es útil si desea acceder a una propiedad/clave pero el nombre se almacena en otra variable, como por ejemplo:

```js
var obj = {
    a: "hello world",
    b: 42
};

var b = "a";

obj[b];            // "hello world"
obj["b"];        // 42
```

Nota: Para obtener más información sobre objetos JavaScript, consulte el título This & Prototype Objects de esta serie, específicamente el Capítulo 3.

Hay un par de otros tipos de valores con los que comúnmente interactúas en los programas JavaScript: `array` y `function`. Pero en lugar de ser  tipos incorporados apropiados, estos deberían ser pensados más como subtipos - versiones especializadas del tipo `object`.

#### Arrays

Un `array` es un objeto que contiene valores \(de cualquier tipo\) particularmente no en las propiedades/claves con nombre, sino en posiciones numéricamente indexadas. Por ejemplo:

```js
var arr = [
    "hello world",
    42,
    true
];

arr[0];            // "hello world"
arr[1];            // 42
arr[2];            // true
arr.length;        // 3

typeof arr;        // "object"
```

Nota: Los lenguajes que empiezan a contar a cero, como JS, utilizan `0` como el índice del primer elemento de la matriz.

Puede ser útil pensar en un `arr` visual:

![](/assets/Captura de pantalla 2017-08-24 a las 9.56.33 p.m..png)

Debido a que los arrays son objetos especiales \(como `typeof` implica\), también pueden tener propiedades, incluyendo la propiedad de longitud actualizada automáticamente.

Teóricamente podría utilizar un array como un objeto normal con sus propias propiedades con nombre, o podría usar un objeto, pero sólo le daría propiedades numéricas \(`0`, `1`, etc.\) similares a un array. Sin embargo, esto generalmente se considera un uso inadecuado de los tipos respectivos.

El  mejor enfoque y más natural es usar arrays para valores numéricamente posicionados y usar objetos para propiedades con nombre.

#### Funciones

El otro subtipo de objeto que usará en todos sus programas JS es una función:

```js
function foo() {
    return 42;
}

foo.bar = "hello world";

typeof foo;            // "function"
typeof foo();        // "number"
typeof foo.bar;        // "string"
```

De nuevo, las funciones son un subtipo de objetos - `typeof` devuelve`"function"`, lo que implica que una función es un tipo principal - y por lo tanto puede tener propiedades, pero normalmente sólo se utilizan las propiedades del objeto `function` \(como `foo.bar`\) en limitadas casos.

Nota: Para obtener más información sobre los valores de JS y sus tipos, consulte los dos primeros capítulos del título de Tipos y Gramática de esta serie.

#### Métodos de tipo incorporado

Los tipos y subtipos incorporados que acabamos de comentar tienen comportamientos predefinidos como propiedades y métodos que son bastante potentes y útiles.

Por ejemplo:

```js
var a = "hello world";
var b = 3.14159;

a.length;                // 11
a.toUpperCase();        // "HELLO WORLD"
b.toFixed(4);            // "3.1416"
```

El "cómo lo hace" detrás de poder llamar `a.toUpperCase()` es más complicado que apenas ese método existente en el valor.

En pocas palabras, hay una forma de envoltorio del objeto `String` \(capitalizar `S`\), normalmente llamada "nativa", que se empareja con el tipo de cadena primitiva; Es este contenedor de objetos que define el método `toUpperCase()` en su prototipo.

Cuando se utiliza un valor primitivo como `"hello world"` como un objeto haciendo referencia a una propiedad o método \(por ejemplo, `a.toUpperCase()` en el fragmento anterior\), JS automáticamente "encaja" el valor a su contraparte wrapper \(escondido bajo cubierta\).

Un valor de `string` puede ser envuelto por un objeto `String`, un `number` puede ser envuelto por un objeto `Number` y un `boolean` puede ser envuelto por un objeto `Boolean`. En la mayoría de las veces, no es necesario preocuparse ni utilizar directamente estas formas de envoltorio de objetos en los valores - se prefieren las formas de valores primitivos en prácticamente todos los casos y JavaScript se encargará del resto por usted.

Nota: Para obtener más información sobre JS nativos y "encajonamiento", consulte el Capítulo 3 del título de Tipos y Gramática de esta serie. Para comprender mejor el prototipo de un objeto, consulte el Capítulo 5 del título this & Object Prototypes de esta serie.

### Comparación de valores

Hay dos tipos principales de comparación de valores que necesitará hacer en sus programas JS: igualdad y desigualdad. El resultado de cualquier comparación es un valor estrictamente booleano \(verdadero o falso\), independientemente de qué tipos de valores se comparan.

#### Coerción

Hablamos brevemente de la coerción en el Capítulo 1, pero vamos a revisarla aquí.

La coerción viene en dos formas en JavaScript: explícita e implícita. La coerción explícita es simplemente que usted puede ver obviamente en el código que una conversión de un tipo a otro ocurrirá, mientras que la coerción implícita es cuando la conversión del tipo puede suceder más como un efecto secundario no obvio de alguna otra operación.

Usted probablemente ha escuchado sentimientos como "la coerción es maligna" sacado del hecho de que hay claramente lugares donde la coerción puede producir algunos resultados sorprendentes. Tal vez nada evoca más la frustración de los desarrolladores que cuando el lenguaje les sorprende.

La coerción no es mala, ni tiene que ser sorprendente. De hecho, la mayoría de los casos puede construir con la coerción de tipo de forma bastante sensata y comprensible, e incluso se puede utilizar para mejorar la legibilidad de su código. Pero no vamos a ir mucho más lejos en ese debate - El Capítulo 4 del título Tipos y Gramática de esta serie cubre todos los lados.

He aquí un ejemplo de coerción explícita:

```js
var a = "42";

var b = Number( a );

a;                // "42"
b;                // 42 -- the number!
```

Y aquí hay un ejemplo de coerción implícita:

```js
var a = "42";

var b = a * 1;    // "42" implicitly coerced to 42 here

a;                // "42"
b;                // 42 -- the number!
```

#### Truthy y Falsy

En el capítulo 1, mencionamos brevemente la naturaleza "verdadera" y "falsa" de los valores: cuando un valor no booleano es coaccionado a un booleano, ¿se convierte en verdadero o falso, respectivamente?

La lista específica de valores falsos en JavaScript es la siguiente:

* `""` \(string vacío\)
* `0`, `-0`, `NaN` \(numero inválido\)
* `null`, `undefined`
* `false`

Cualquier valor que no esté en esta lista falsa es "`true`". Estos son algunos ejemplos:

* `"hello"`
* `42`
* `true`
* `[ ]`,`[ 1, "2", 3 ]`\(arrays\)
* `{ }`,`{ a: 42 }`\(objects\)
* `function foo() { .. }`\(functions\)

Es importante recordar que un valor no booleano sólo sigue esta coerción "`true`" / "`false`" si es realmente coaccionado a un booleano. No es tan difícil confundirse con una situación que parece que coacciona un valor a un booleano cuando no lo es.

#### Igualdad

Hay cuatro operadores de igualdad: `==`, `===`, `!=`, Y `!==`. Las forma `!` son, por supuesto, las versiones simétricas "no iguales" de sus contrapartes; La no igualdad no debe confundirse con la desigualdad.

La diferencia entre `==` y `===` normalmente se caracteriza por que `==` comprueba la igualdad de valor y` ===` comprueba tanto el valor como la igualdad de tipo. Sin embargo, esto es inexacto. La manera correcta de caracterizarlos es que `==` chequea por igualdad de valor con coerción permitida, y `===` comprueba la igualdad de valor sin permitir coerción; `===` se llama a menudo "igualdad estricta" por esta razón.

Considere la coerción implícita que es permitida por la comparación `==` loose-equality y no se permite con la `===` estricta-igualdad:

```js
var a = "42";
var b = 42;

a == b;			// true
a === b;		// false
```

En la comparación `a == b`, JS advierte que los tipos no coinciden, por lo que pasa por una serie ordenada de pasos para coaccionar uno o ambos valores a un tipo diferente hasta que los tipos coincidan, donde entonces se puede comprobar una igualdad de valor simple .

Si piensas en ello, hay dos formas posibles de que `a == b` puedan dar `true` a través de coerción. O la comparación podría terminar como `42 == 42` o podría ser `"42" == "42"`. Entonces, ¿cuál es?

La respuesta: `"42"` se convierte en `42`, para hacer la comparación `42 == 42`. En un ejemplo tan simple, en realidad no parece importar de qué manera va ese proceso, ya que el resultado final es el mismo. Hay casos más complejos donde importa no sólo cuál es el resultado final de la comparación, sino cómo llegar allí.

El `a === b` produce `false`, porque la coerción no está permitida, por lo que la comparación de valores sencillos obviamente falla. Muchos desarrolladores sienten que `===` es más predecible, por lo que abogan por usar siempre esa forma y mantenerse alejados de `==`. Creo que esta visión es muy miope. Creo que `==` es una poderosa herramienta que ayuda a su programa, si usted se toma el tiempo para aprender cómo funciona.

No vamos a cubrir todos los detalles básicos de cómo la coerción en comparaciones funciona aquí. Mucho de él es bastante sensible, pero hay algunos casos importantes a tener cuidado. Puede leer la sección 11.9.3 de la especificación ES5 \([http://www.ecma-international.org/ecma-262/5.1/](http://www.ecma-international.org/ecma-262/5.1/)\) para ver las reglas exactas, y le sorprenderá lo sencillo que es este mecanismo , En comparación con todo el bombo negativo que lo rodea.

Para reducir un montón de detalles a unos pocos takeaways simples, y ayudarle a saber si utilizar `==` o `===` en varias situaciones, aquí están mis reglas simples:

* Si un valor \(aka lado\) en una comparación podría ser el valor `true` o `false`, evite `==` y utilice `===`.
* Si cualquier valor en una comparación podría ser uno de estos valores específicos \(`0`, `""`, o `[]` - matriz vacía\), evite `==` y utilice `===`.
* En todos los demás casos, es seguro utilizar `==`. No sólo es seguro, sino que en muchos casos simplifica su código de una manera que mejora la legibilidad.

A lo que estas reglas se reducen es a exigir que usted piense críticamente sobre su código y sobre qué tipos de valores pueden venir a través de las variables que se comparan por igualdad. Si puede estar seguro acerca de los valores, y `==` es seguro, úselo! Si no puede estar seguro acerca de los valores, utilice `===`. Es así de simple.

El `!=` No-igualdad forma pares con `==`, y el `!==` pares de forma con `===`. Todas las reglas y observaciones que acabamos de discutir se mantienen simétricamente para estas comparaciones sin igualdad.

Debe tomar nota especial de las reglas de comparación `==` y `===` si está comparando dos valores no primitivos, como objetos \(incluyendo funciones y arrays\). Debido a que esos valores se mantienen en realidad por referencia, las comparaciones `==` y `===` simplemente comprobarán si las referencias coinciden, no cualquier cosa acerca de los valores subyacentes.

Por ejemplo, las matrices se coaccionan por defecto a strings simplemente uniendo todos los valores con comas \(`,`\) . Podría pensar que dos matrices con el mismo contenido serían `==` iguales, pero no lo son:

```js
var a = [1,2,3];
var b = [1,2,3];
var c = "1,2,3";

a == c;		// true
b == c;		// true
a == b;		// false
```

Nota: Para obtener más información acerca de las reglas de comparación de igualdad, consulte la especificación ES5 \(sección 11.9.3\) y consulte también el Capítulo 4 del título de Tipos y Gramática de esta serie; Consulte el Capítulo 2 para obtener más información acerca de valores versus referencias.

#### Desigualdad

Los operadores `<`, `>`, `<=`, y `>=` se utilizan para la desigualdad, a la que se hace referencia en la especificación como "comparación relacional". Normalmente se utilizarán con valores comparables como números. Es fácil entender que `3 < 4`.

Pero los valores `string` de JavaScript también se pueden comparar por desigualdad, usando reglas alfabéticas típicas \(`"bar" < "foo"`\).

¿Y la coerción? Se usan reglas similares como con la comparación `==`  \(aunque no exactamente idénticas!\) a los operadores de desigualdad. Cabe destacar que no hay operadores de "desigualdad estricta" que desautorizarían la coerción de la misma manera que la "igualdad estricta".

Considere:

```js
var a = 41;
var b = "42";
var c = "43";

a < b;		// true
b < c;		// true
```

¿Qué pasa aquí? En la sección 11.8.5 de la especificación ES5, se dice que si ambos valores en la comparación `<` son strings, como ocurre con `b < c`, la comparación se hace lexicográficamente \(aka alfabéticamente como un diccionario\). Pero si uno o ambos no es una cadena, como ocurre con `a < b`, entonces ambos valores son forzados a ser números y se produce una comparación numérica típica.

El gotcha más grande que se puede encontrar aquí con comparaciones entre tipos de valor potencialmente diferentes - recuerde, no hay formas de "desigualdad estricta" para usar - es cuando uno de los valores no se puede convertir en un número válido, como:

```js
var a = 42;
var b = "foo";

a < b;		// false
a > b;		// false
a == b;		// false
```

Espera, ¿cómo pueden ser falsas las tres comparaciones? Debido a que el valor `b` está siendo coaccionado al "valor de número inválido" `NaN` en las comparaciones `<` y `>`, y la especificación dice que `NaN` no es ni mayor ni menor que cualquier otro valor.

La comparación `==` falla por una razón diferente. `a == b` podría fallar si se interpreta como `42 == NaN` o `"42" == "foo"` - como hemos explicado anteriormente, el primero es el caso.

Nota: Para obtener más información sobre las reglas de comparación de desigualdades, consulte la sección 11.8.5 de la especificación ES5 y consulte también el Capítulo 4 del título Tipos y Gramática de esta serie.

