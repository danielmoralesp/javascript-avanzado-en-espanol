# 7.2 Bloques implícitos vs. explícitos

En el capítulo 3, hemos identificado algunos peligros potenciales para la capacidad de mantenimiento / refactorabilidad del código cuando introducimos el ámbito de bloque. ¿Hay otra manera de aprovechar el ámbito de bloque pero para reducir este inconveniente?

Considere esta forma alternativa de `let`, llamada "let block" o "let statement" \(en contraste con "let declarations" de antes\).

```js
let (a = 2) {
	console.log( a ); // 2
}

console.log( a ); // ReferenceError
```

En lugar de implícitamente secuestrar un bloque existente, la instrucción `let` crea un bloque explícito para su vinculación de ámbito. No sólo el bloque explícito se destaca más, y tal vez más robusto en la refactorización de código, produce un código más limpio, gramaticamente, forzando todas las declaraciones a la parte superior del bloque. Esto hace que sea más fácil mirar a cualquier bloque y saber cuál es scoped a ella y cual no.

Como patrón, refleja el enfoque que muchas personas adoptan en el scope de funciones cuando mueven / elevan manualmente todas sus declaraciones `var` a la parte superior de la función. El let-statement los coloca allí en la parte superior del bloque por intención, y si no usas las declaraciones diseminadas, tus declaraciones de alcance de bloque son algo más fáciles de identificar y mantener.

Pero, hay un problema. El formulario let-statement no está incluido en ES6. Tampoco el compilador oficial de Traceur acepta esa forma de código.

Tenemos dos opciones. Podemos formatearlo usando la sintaxis válida de ES6 y un poco de disciplina en el código:

```js
/*let*/ { let a = 2;
	console.log( a );
}

console.log( a ); // ReferenceError
```

Pero, las herramientas están destinadas a resolver nuestros problemas. Por lo tanto, la otra opción es escribir bloques explícitos de instrucciones y dejar que una herramienta los convierta en código válido de trabajo.

Por lo tanto, he construido una herramienta llamada "let-er" \[^ note-let\_er\] para abordar sólo este problema. Let-er es un transpilador de código de build-step, pero su única tarea es encontrar formularios let-statement y transpilarlos. Dejará solo cualquiera del resto de su código, incluyendo cualquier let-declaraciones. Puede usar let-er como el primer paso del transpilador ES6 y, a continuación, pasar su código a través de algo como Traceur si es necesario.

Además, let-er tiene un indicador de configuración --es6, que cuando se enciende \(desactivado por defecto\), cambia el tipo de código producido. En lugar del `try` / `catch` ES3 polyfill hack, let-er tomaría nuestro fragmento y produce uno totalmente compatible con ES6, no hacky:

```js
{
	let a = 2;
	console.log( a );
}

console.log( a ); // ReferenceError
```

Por lo tanto, puede empezar a utilizar let-er de inmediato y orientar todos los entornos pre-ES6, y cuando sólo se preocupan por ES6, puede agregar el indicador y orientar instantáneamente sólo ES6.

Y lo más importante, puede utilizar el formulario de declaración de instrucciones más preferible y más explícito aunque no sea parte oficial de ninguna versión de ES \(todavía\).

