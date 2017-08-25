# 2.3 Condicionales

Además de la declaración `if` que introdujimos brevemente en el Capítulo 1, JavaScript proporciona algunos otros mecanismos condicionales a los que deberíamos echar un vistazo.

A veces se puede encontrar escribiendo una serie de declaraciones `if..else..if` como esta:

```js
if (a == 2) {
	// do something
}
else if (a == 10) {
	// do another thing
}
else if (a == 42) {
	// do yet another thing
}
else {
	// fallback to here
}
```

Esta estructura funciona, pero es un poco verbosa porque es necesario especificar la comparación `a` para cada caso. Aquí hay otra opción, la instrucción `switch`:

```js
switch (a) {
	case 2:
		// do something
		break;
	case 10:
		// do another thing
		break;
	case 42:
		// do yet another thing
		break;
	default:
		// fallback to here
}
```

El `break` es importante si sólo desea que se ejecute la instrucción\(es\) en un `case` \(caso\). Si omite el `break` de un `case`, y ese `case` coincide o se ejecuta, la ejecución continuará con las declaraciones del `case` siguiente, independientemente de la coincidencia de ese `case`. A veces, este llamado "caer a través \(fall through\)" es útil/deseado:

```js
switch (a) {
	case 2:
	case 10:
		// some cool stuff
		break;
	case 42:
		// other stuff
		break;
	default:
		// fallback
}
```

Aquí, si `a` es `2` o `10`, ejecutará el código "// some cool stuff"

Otra forma de condicional en JavaScript es el "operador condicional", a menudo llamado el "operador ternario". Es como una forma más concisa de una sola declaración `if..else`, como:

```js
var a = 42;

var b = (a > 41) ? "hello" : "world";

// similar to:

// if (a > 41) {
//    b = "hello";
// }
// else {
//    b = "world";
// }
```

Si la expresión de prueba \(`a > 41` aquí\) se evalúa como verdadera, se obtiene la primera cláusula \(`"hola"`\), de lo contrario se obtiene la segunda cláusula \(`"mundo"`\).

El operador condicional no tiene que ser utilizado en una asignación, pero eso es definitivamente el uso más común.

Nota: Para obtener más información sobre las condiciones de prueba y otros patrones de `switch` y `? :`, Vea el título Tipos y Gramática de esta serie.

