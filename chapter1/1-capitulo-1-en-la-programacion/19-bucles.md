# 1.9 Bucles

Durante las horas pico, hay una lista de espera para los clientes que necesitan hablar con el empleado de la tienda telefónica. Aunque todavía hay personas en esa lista, sólo tiene que seguir sirviendo al próximo cliente.

Repetir un conjunto de acciones hasta que cierta condición falla - en otras palabras, repetir sólo mientras la condición se mantiene - es el trabajo de los bucles de programación; Los bucles pueden tomar diferentes formas, pero todos satisfacen este comportamiento básico.

Un bucle incluye la condición de prueba, así como un bloque \(normalmente como `{..}`\). Cada vez que se ejecuta el bloque del bucle, se llama iteración.

Por ejemplo, el bucle `while` y las formas de bucle `do..while` ilustran el concepto de repetir un bloque de sentencias hasta que una condición ya no se evalúa como verdadera:

```js
while (numOfCustomers > 0) {
	console.log( "How may I help you?" );

	// help the customer...

	numOfCustomers = numOfCustomers - 1;
}

// versus:

do {
	console.log( "How may I help you?" );

	// help the customer...

	numOfCustomers = numOfCustomers - 1;
} while (numOfCustomers > 0);
```

La única diferencia práctica entre estos bucles es si el condicional se prueba antes de la primera iteración \(`while`\) o después de la primera iteración \(`do..while`\).

De cualquier forma, si las pruebas condicionales son falsas, la siguiente iteración no se ejecutará. Esto significa que si la condición es inicialmente falsa, un bucle while nunca se ejecutará, pero un bucle `do..while` se ejecutará sólo la primera vez.

A veces usted está haciendo un bucle con el propósito de contar un cierto conjunto de números, como de 0 a 9 \(diez números\). Puede hacerlo estableciendo una variable de iteración de bucle como `i` en el valor `0` e incrementándola en `1` cada iteración.

Advertencia: Por una variedad de razones históricas, los lenguajes de programación casi siempre cuentan las cosas de manera cero, es decir, comenzando con 0 en lugar de 1. Si no está familiarizado con ese modo de pensar, puede ser muy confuso al principio. Tómese su tiempo para practicar el conteo empezando por 0 para sentirse más cómodo con él.

El condicional se prueba en cada iteración, como si hubiera una declaración implícita `if `dentro del bucle.

Podemos usar la instrucción `break` de JavaScript para detener un bucle. También, podemos observar que es extremadamente fácil crear un bucle que de otra manera funcionaría para siempre sin un mecanismo para detenerla.

Vamos a ilustrar esto:

```js
var i = 0;

// a `while..true` loop would run forever, right?
while (true) {
	// stop the loop?
	if ((i <= 9) === false) {
		break;
	}

	console.log( i );
	i = i + 1;
}
// 0 1 2 3 4 5 6 7 8 9
```

Advertencia: esto no es una forma práctica en la que desearía utilizar sus bucles. Se presenta aquí sólo con fines ilustrativos.

Mientras que un `while` \(o `do..while`\) puede realizar la tarea manualmente, hay otra forma sintáctica llamada un bucle `for` sólo para ese propósito:

```js
for (var i = 0; i <= 9; i = i + 1) {
	console.log( i );
}
// 0 1 2 3 4 5 6 7 8 9
```

Como puede ver, en ambos casos el condicional` i <= 9` es verdadero para las primeras 10 iteraciones \(`i `de valores de 0 a 9\) de cualquier forma de bucle, pero se convierte en falso una vez que `i` es un valor 10.

El bucle `for` tiene tres cláusulas: la cláusula de inicialización \(`var i = 0`\), la cláusula de prueba condicional \(`i <= 9`\) y la cláusula de actualización \(`i = i + 1`\). Así que si vas a contar con tus iteraciones de bucle, es una forma más compacta ya menudo más fácil de entender y escribir.

Existen otros formularios de bucle especializados que tienen la intención de iterar sobre valores específicos, como las propiedades de un objeto \(véase el capítulo 2\), donde la prueba condicional implícita es justamente si se han procesado todas las propiedades. El concepto de "bucle hasta que falla una condición" no importa cuál sea la forma del bucle.



