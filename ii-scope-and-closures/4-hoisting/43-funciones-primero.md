# 4.3 Funciones Primero

Se sube tanto las declaraciones de funciones como las declaraciones de variables. Pero un detalle sutil \(que puede aparecer en el código con varias declaraciones "duplicadas"\) es que las funciones se suben primero y luego las variables.

Considere:

```js
foo(); // 1

var foo;

function foo() {
	console.log( 1 );
}

foo = function() {
	console.log( 2 );
};
```

`1` se imprime en lugar de `2`! Este fragmento es interpretado por el Motor como:

```js
function foo() {
	console.log( 1 );
}

foo(); // 1

foo = function() {
	console.log( 2 );
};
```

Tenga en cuenta que `var foo` era la declaración duplicada \(y por lo tanto ignorada\), aunque llegó antes de la declaración `function foo() ...` , porque las declaraciones de función son subidas antes que las variables normales.

Mientras las declaraciones `var` multiples/duplicadas se ignoran de forma efectiva, las declaraciones de función posteriores reemplazan a las anteriores.

```js
foo(); // 3

function foo() {
	console.log( 1 );
}

var foo = function() {
	console.log( 2 );
};

function foo() {
	console.log( 3 );
}
```

Si bien esto todo puede sonar como nada más que curiosidades académicas interesantes, pone de relieve el hecho de que las definiciones duplicadas en el mismo alcance son una idea realmente mala y, a menudo llevará a resultados confusos.

Las declaraciones de funciones que aparecen dentro de los bloques normales típicamente se elevan al ámbito de inclusión, en lugar de ser condicionales como implica este código:

```js
foo(); // "b"

var a = true;
if (a) {
   function foo() { console.log( "a" ); }
}
else {
   function foo() { console.log( "b" ); }
}
```

Sin embargo, es importante tener en cuenta que este comportamiento no es fiable y está sujeto a cambios en futuras versiones de JavaScript, por lo que es mejor evitar declarar funciones en bloques.



