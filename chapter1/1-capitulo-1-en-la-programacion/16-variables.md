# 1.6 Variables

La mayoría de los programas útiles necesitan rastrear un valor a medida que cambia a lo largo del programa, pasando por diferentes operaciones según lo solicitado por las tareas previstas de su programa.

La forma más sencilla de hacerlo en su programa es asignar un valor a un contenedor simbólico, llamado variable, llamado así porque el valor en este contenedor puede variar con el tiempo según sea necesario.

En algunos lenguajes de programación, se declara una variable \(contenedor\) para contener un tipo específico de valor, como número o cadena. La tipificación estática, también conocida como ejecución de tipo, se cita típicamente como un beneficio para la corrección del programa al evitar conversiones de valor no deseadas.

Otros idiomas enfatizan tipos para valores en lugar de variables. La tipificación débil, también conocida como escritura dinámica, permite que una variable contenga cualquier tipo de valor en cualquier momento. Se suele citar como un beneficio para la flexibilidad del programa al permitir que una única variable represente un valor sin importar la forma de tipo que el valor pueda tomar en un momento dado en el flujo lógico del programa.

JavaScript utiliza el último enfoque, el tipo dinámico, lo que significa que las variables pueden contener valores de cualquier tipo sin ningún tipo de aplicación.

Como se mencionó anteriormente, declaramos una variable usando la instrucción `var` - note que no hay otra información de tipo en la declaración. Considere este sencillo programa:

```js
var amount = 99.99;

amount = amount * 2;

console.log( amount );		// 199.98

// convert `amount` to a string, and
// add "$" on the beginning
amount = "$" + String( amount );

console.log( amount );		// "$199.98"
```

La variable `amount` comienza teniendo el número 99.99, y luego tiene el resultado del número de la cantidad \* 2, que es 199.98.

El primer comando console.log \(..\) tiene que coaccionar implícitamente ese valor numérico a una cadena para imprimirlo.

Entonces el valor de la sentencia `= "$" + String (amount)` coacciona explícitamente el valor 199.98 a una cadena y agrega un carácter "$" al principio. En este punto, `amount` ahora tiene el valor de cadena "$ 199.98", por lo que la segunda instrucción console.log \(..\) no necesita hacer ninguna coerción para imprimirlo.

Los desarrolladores de JavaScript notarán la flexibilidad de usar la variable `amount` para cada uno de los valores de 99.99, 199.98 y "$ 199.98". Los entusiastas de la tipificación estática preferirían una variable independiente como `amountStr` para mantener la representación final de "$ 199.98" del valor, porque es un tipo diferente.

De cualquier manera, notará que `amount` tiene un valor de ejecución que cambia a lo largo del programa, ilustrando el propósito principal de las variables: administrar el estado del programa.

En otras palabras, `state` es el seguimiento de los cambios a los valores a medida que su programa se ejecuta.

Otro uso común de variables es para centralizar la configuración de valores. Esto se llama más comúnmente constantes, cuando usted declara una variable con un valor y la intención es que el valor no cambie a través del programa.

Usted declara estas constantes, a menudo en la parte superior de un programa, por lo que es conveniente para usted tener un lugar para ir a alterar un valor si es necesario. Por convención, las variables JavaScript como constantes suelen ser mayúsculas, con guiones bajos \_ entre varias palabras.

He aquí un ejemplo tonto:

```js
var TAX_RATE = 0.08;	// 8% sales tax

var amount = 99.99;

amount = amount * 2;

amount = amount + (amount * TAX_RATE);

console.log( amount );				// 215.9784
console.log( amount.toFixed( 2 ) );	// "215.98"
```

Nota: Similar a cómo `console.log (..)` es un registro de `function(..)` que se accede como una propiedad de objeto en el valor de la consola, `toFixed(..)` aquí es una función a la que se puede acceder en valores numéricos. Los números de JavaScript no se formatean automáticamente para dólares - el motor no sabe cuál es su intención y no hay un tipo de moneda. `ToFixed(..)` nos permite especificar cuántos números decimales queremos que el número sea redondeado, y produce la cadena según sea necesario.

La variable `TAX_RATE` es sólo constante por convención - no hay nada especial en este programa que impide que se cambie. Pero si la ciudad eleva la tasa del impuesto a las ventas al 9%, podemos actualizar fácilmente nuestro programa estableciendo el valor `TAX_RATE` asignado a 0,09 en un lugar, en lugar de buscar y encontrar muchas ocurrencias del valor 0.08 puesto a lo largo del programa y actualizandolo en todas ellas .

La versión más reciente de JavaScript en el momento de escribir este documento \(comúnmente llamado "ES6"\) incluye una nueva forma de declarar constantes, utilizando `const` en lugar de `var`:

```js
// as of ES6:
const TAX_RATE = 0.08;

var amount = 99.99;

// ..
```

Las constantes son útiles al igual que las variables con valores sin cambios, excepto que las constantes también evitan el cambio accidental de valor en algún otro lugar después de la configuración inicial. Si intentó asignar un valor diferente a `TAX_RATE` después de esa primera declaración, su programa rechazaría el cambio \(y en modo estricto, fallará con un error; consulte "Modo estricto" en el Capítulo 2\).

Por cierto, ese tipo de "protección" contra los errores es similar al tipo de tipificación estática, por lo que puede ver por qué los tipos estáticos en otros idiomas pueden ser atractivos!

Nota: Para obtener más información sobre cómo se pueden utilizar diferentes valores en las variables en sus programas, consulte el título Tipos y Gramática de esta serie.



