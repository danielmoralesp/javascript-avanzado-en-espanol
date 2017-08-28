# 7- Ámbito de bloque de Polyfilling

En el Capítulo 3, exploramos el Ámbito de Bloque. Vimos que `with` y la cláusula `catch` son ejemplos minúsculos de ámbito de bloque que han existido en JavaScript al menos desde la introducción de ES3.

Pero es la introducción de ES6 de `let` que finalmente brinda capacidad de alcance de bloque completo y sin restricciones a nuestro código. Hay muchas cosas interesantes, tanto funcionales como de estilo de código, que permitirán el alcance de bloque.

Pero, ¿qué pasaría si quisieramos utilizar el ámbito de bloque en los entornos pre-ES6?

Considere este código:

```js
{
	let a = 2;
	console.log( a ); // 2
}

console.log( a ); // ReferenceError
```

Esto funcionará muy bien en entornos ES6. Pero, ¿podemos hacerlo con las versiones anteriores de ES6? `catch` es la respuesta.

```js
try{throw 2}catch(a){
	console.log( a ); // 2
}

console.log( a ); // ReferenceError
```

Whoa! Ese es un código feo, extraño. Vemos un try / catch que parece arrojar un error forzosamente, pero el "error" que lanza es sólo un valor `2`, y luego la declaración de variables que lo recibe está en la cláusula `catch(a)`. Mente: explota!.

Así es, la cláusula `catch` tiene un alcance de bloque, lo que significa que puede usarse como un polyfill para el ámbito de bloque en entornos pre-ES6.

"Pero ...", dices. "... nadie quiere escribir un código así de feo!" Es verdad. Nadie escribe \(algunos\) el código generado por el compilador de CoffeeScript, tampoco. Ese no es el punto.

El punto es que las herramientas pueden transpilar el código ES6 para trabajar en entornos pre-ES6. Puede escribir código utilizando el ámbito de bloque y beneficiarse de dicha funcionalidad y dejar que una herramienta de creación de pasos se encargue de producir código que funcionará de verdad cuando se implementa.

Éste es realmente el camino de migración preferido para todos \(ahem, la mayoría\) de ES6: usar un transpilador de código para tomar código ES6 y producir código compatible con ES5 durante la transición de pre-ES6 a ES6.



