# 1.5 Revisión \(TL; DR\)

El Scope es un conjunto de reglas que determina dónde y cómo se puede buscar una variable \(identificador\). Esta búsqueda puede ser con el propósito de asignar a la variable, que es una referencia LHS \(izquierda\), o puede ser con el propósito de recuperar su valor de referencia, que es un RHS \(lado derecho\).

Las referencias LHS resultan de las operaciones de asignación. Las asignaciones relacionadas con el ámbito pueden ocurrir con el operador `=` o pasando argumentos a \(asignar a\) parámetros de función.

El Motor de JavaScript primero compila el código antes de que se ejecute, y al hacerlo, divide declaraciones como `var a = 2`; En dos pasos separados:

1. Primero, `var a` para declararlo en ese ámbito. Esto se realiza al principio, antes de la ejecución del código.
2. Después, `a = 2` para buscar la variable \(referencia LHS\) y asignarla si se encuentra.

Las consultas de referencia de LHS y RHS comienzan en el ámbito actualmente en ejecución, y si es necesario \(es decir, no encuentran lo que buscan allí\), siguen su camino hasta el ámbito anidado, un ámbito \(piso\) a la vez, buscando el identificador, hasta que lleguen al global \(piso superior\) y se detengan, y lo encuentren, o no lo hagan.

Las referencias no encontradas de RHS dan lugar a la emisión de `ReferenceErrors`. Las referencias no cumplidas de LHS resultan en una variable global automática creada implícitamente de ese nombre \(si no en "Strict Mode" \[^note-strictmode\]\), o un `ReferenceError` \(si en "Strict Mode" \[^note-strictmode\]\).

### Respuestas del Examen

```js
function foo(a) {
	var b = a;
	return a + b;
}

var c = foo( 2 );
```

1. Identifique todas las busquedas LHS \(hay 3!\).

   `c = ..` , `a = 2` \(asignación implícita de parámetros\) y `b = ..`
2. Identificar todas las consultas RHS \(hay 4!\)

   `foo(2..` , `= a;` , `a + ..` y `.. + b`

\[^note-strictmode\]: MDN: [Strict Mode](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions_and_function_scope/Strict_mode)

