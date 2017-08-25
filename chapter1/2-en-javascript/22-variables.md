# 2.2 Variables

En JavaScript, los nombres de las variables \(incluidos los nombres de las funciones\) deben ser identificadores válidos. Las reglas estrictas y completas para caracteres válidos en identificadores son un poco complejas cuando se consideran caracteres no tradicionales como Unicode. Sin embargo, si sólo se consideran caracteres alfanuméricos ASCII típicos, las reglas son simples.

Un identificador debe comenzar con `a-z`, `A-Z`, `$` o `_`. Entonces puede contener cualquiera de esos caracteres más los números `0-9`.

Generalmente, las mismas reglas se aplican a un nombre de propiedad como a un identificador de variable. Sin embargo, ciertas palabras no se pueden utilizar como variables, pero son correctas como nombres de propiedad. Estas palabras se llaman "palabras reservadas" e incluyen las palabras clave JS \(`for`, `in`, `if`, etc.\) así como `null`, `true` y `false`.

Nota: Para obtener más información sobre las palabras reservadas, consulte el Apéndice A del título Tipos y Gramática de esta serie.

### Ámbitos \(scope\) de las funciones

Utilice la palabra clave `var` para declarar una variable que pertenecerá al ámbito de la función actual o al ámbito global si está en el nivel superior fuera de cualquier función.

#### Hoisting \(Elevación\)

Siempre que aparezca una `var` dentro de un ámbito, esa declaración se toma para pertenecer a todo el ámbito y es accesible en todas partes.

Metafóricamente, este comportamiento se denomina _**hoisting**_, cuando una declaración `var` es conceptualmente "movida" a la parte superior de su ámbito de inclusión. Técnicamente, este proceso se explica con más precisión por la forma en que se compila el código, pero podemos omitir esos detalles por ahora.

Considerar:

```js
var a = 2;

foo();	// funciona porque la declaracion `foo()` esta "hoisted" (elevada). O sea que primero ha sido llamada 
       // la función y después es declarada

function foo() {
	a = 3;

	console.log( a );	// 3

	var a;		// la declaración esta "hoisted" (elevada) en la parte superior de `foo()`
}

console.log( a );	// 2
```

**Advertencia**: No es común o una buena idea confiar en la elevación \(hoisting\) de la variable para usar esa variable en su alcance que aparece en su declaración var; Puede ser bastante confuso. Es mucho más común y aceptado el uso de declaraciones de funciones _hoisting_, como lo hacemos con la llamada `foo()` que aparece antes de su declaración formal.

#### Ámbitos anidados

Cuando declara una variable, ella estará disponible en cualquier parte de ese ámbito, así como en cualquier ámbito inferior/interno. Por ejemplo:

```js
function foo() {
    var a = 1;

    function bar() {
        var b = 2;

            function baz() {
                var c = 3;

                console.log( a, b, c );	// 1 2 3
            }

            baz();
            console.log( a, b );		// 1 2
    }

    bar();
    console.log( a );				// 1
}

foo();
```

Observe que `c` no está disponible dentro de `bar()`, porque se declara sólo dentro del ámbito interno `baz()` y que `b` no está disponible para `foo()` por la misma razón.

Si intenta acceder al valor de una variable en un ámbito en el que no está disponible, obtendrá un `ReferenceError` . Si intenta establecer una variable que no ha sido declarada, o bien terminará creando una variable en el ámbito global de nivel superior \(¡mal!\) O recibiendo un error, dependiendo del "modo estricto" \(consulte "Modo estricto "\). Vamos a ver:

```js
function foo() {
	a = 1;	// `a` no esta formalmente declarada con `var`
}

foo();
a;			// 1 -- oops, variable automatica global :(
```

Esta es una muy mala práctica. ¡No lo hagas! Siempre declare formalmente sus variables.

Además de crear declaraciones para variables en el nivel de función, ES6 le permite declarar que las variables pertenecen a bloques individuales \(pares de `{..}`\), usando la palabra clave `let`. Además de algunos detalles matizados, las reglas de alcance se comportarán aproximadamente iguales a las que acabamos de ver con funciones:

```js
function foo() {
	var a = 1;

	if (a >= 1) {
		let b = 2;

		while (b < 5) {
			let c = b * 2;
			b++;

			console.log( a + c );
		}
	}
}

foo();
// 5 7 9
```

Debido al uso de `let` en lugar de `var`,  `b` pertenecerá sólo a la instrucción `if` y, por tanto, no al ámbito de la función `foo()`. Del mismo modo, `c` sólo pertenece al bucle `while`. El ámbito de bloque es muy útil para administrar sus ámbitos variables de una manera más refinada, lo que puede hacer que su código sea mucho más fácil de mantener con el tiempo.

Nota: Para obtener más información sobre los ámbitos, consulte el título Scope & Closures de esta serie. Consulte el título ES6 & Beyond de esta serie para obtener más información sobre el ámbito de bloque `let`.

