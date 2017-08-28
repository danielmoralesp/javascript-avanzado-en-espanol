# 5.3 Ahora puedo ver

Los fragmentos de código anteriores son algo académicos y construidos artificialmente para ilustrar el uso del closures. Pero te prometí algo más que un juguete nuevo. Le prometí que el closure era algo que aparecía constantemente a su alrededor en su código existente. Veamos ahora esa verdad.

```js
function wait(message) {

	setTimeout( function timer(){
		console.log( message );
	}, 1000 );

}

wait( "Hello, closure!" );
```

Tomamos una función interna \(llamada `timer`\) y la pasamos a `setTimeout(..)`. Pero el temporizador tiene un alcance de closure sobre el alcance de `wait(..)`, de hecho mantiene y utiliza una referencia a la variable `message`.

Mil milisegundos después de que hayamos ejecutado `wait(..)`, y su alcance interior de otra manera se hubiera ido mucho tiempo, ese temporizador de la función interna todavía tiene un closure sobre ese alcance.

En el fondo de las entrañas del Motor, la utilidad incorporada `setTimeout(...)` hace referencia a algún parámetro, probablemente llamado `fn` o `func` o algo así. El motor invoca esa función, que está invocando nuestra función de temporizador interno, y la referencia de alcance léxico está intacta.

### Closure.

O, si usted esta persuadido por jQuery \(o cualquier framework JS, para el caso\):

```js
function setupBot(name,selector) {
	$( selector ).click( function activator(){
		console.log( "Activating: " + name );
	} );
}

setupBot( "Closure Bot 1", "#bot_1" );
setupBot( "Closure Bot 2", "#bot_2" );
```

No estoy seguro de qué tipo de código escribes, pero regularmente escribo código que es el responsable de controlar un ejército de drones global lleno de bots de closures, ¡así que esto es totalmente realista!

\(Algunos\) bromas aparte, esencialmente siempre y dondequiera que traten las funciones \(que acceden a sus propios ámbitos léxicos respectivos\) como valores de primera clase y los pasan alrededor, es probable que vean las funciones de ejercicio de closures. Sea que los temporizadores, los manejadores de eventos, las solicitudes de Ajax, la mensajería de ventanas cruzadas, los workers de la web, o cualquiera de las otras tareas asíncronas \(o síncronas!\), Al pasar en una función de devolución de llamada, prepárate para el sling alrededor de los closures!

**Nota**: El capítulo 3 introdujo el patrón IIFE. Aunque a menudo se dice que el IIFE \(solo\) es un ejemplo de closure observado, estaría algo en desacuerdo, por nuestra definición anterior.

```js
var a = 2;

(function IIFE(){
	console.log( a );
})();
```

Este código "funciona", pero no es estrictamente una observación de como funcionan los closures. ¿Por qué? Debido a que la función \(que denominamos "IIFE" aquí\) no se ejecuta fuera de su alcance léxico. Todavía se invoca allí mismo en el mismo ámbito que se declaró \(el ámbito global que también contiene `a`\).  `a` se encuentra a través de la búsqueda de alcance léxico normal, no realmente a través del closure.

Si bien el closure podría estar técnicamente ocurriendo a la hora de la declaración, no es estrictamente observable, y así, como dicen, es un árbol que cae en el bosque sin que nadie lo oiga.

Aunque un IIFE no es en sí mismo un ejemplo de closure, crea absolutamente el alcance, y es una de las herramientas más comunes que utilizamos para crear un ámbito que se puede cerrar. Por lo tanto, los IIFEs están fuertemente relacionados con los closures, incluso si no ejercen el closure ellos mismos.

Ponga este libro abajo en este momento, querido lector. Tengo una tarea para ti. Vaya a abrir parte de su código JavaScript reciente. Busque sus funciones como valores e identifique dónde está utilizando el closure y tal vez ni siquiera lo sabía antes.

Esperaré.

¡Ahora lo ves!

