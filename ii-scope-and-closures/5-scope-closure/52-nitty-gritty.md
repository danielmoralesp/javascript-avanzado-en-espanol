# 5.2 Nitty Gritty

OK, suficiente hipérbole y referencias de películas desvergonzadas.

Aquí está una definición de lo que usted necesita saber para entender y reconocer los closures:

> El closure es cuando una función es capaz de recordar y acceder a su ámbito léxico incluso cuando esa función está ejecutándose fuera de su ámbito léxico.

Vamos a ver algo de código para ilustrar esa definición.

```js
function foo() {
	var a = 2;

	function bar() {
		console.log( a ); // 2
	}

	bar();
}

foo();
```



