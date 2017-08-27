# 2.1 Tiempo de Lex

Como discutimos en el capítulo 1, la primera fase tradicional de un Compilador de lenguaje estándar se llama lexing \(aka, tokenizing\). Si recuerda, el proceso de lexing examina una cadena de caracteres de código fuente y asigna significado semántico a los tokens como resultado de algún análisis de estado.

Es este concepto el que proporciona la base para entender qué es el ámbito léxico y de dónde proviene el nombre.

Para definirlo, el ámbito léxico es el alcance que se define en el tiempo de lexing. En otras palabras, el ámbito léxico se basa en donde son escritas por usted las variables y los bloques de ámbito, en tiempo de escritura, y por lo tanto es \(en su mayoría\) establecidos invariablemente en el momento en que el lexer procesa su código.

**Nota**: Vamos a ver en un poco que hay algunas maneras de engañar el ámbito léxico, por lo tanto, podemos modificarlo después de que el lexer ha pasado, pero esto es algo mal visto. Se considera la mejor práctica para tratar el ámbito léxico como, de hecho, sólo léxico, y por lo tanto totalmente invariable en el tiempo por naturaleza.

Consideremos este bloque de código:

```js
function foo(a) {

	var b = a * 2;

	function bar(c) {
		console.log( a, b, c );
	}

	bar(b * 3);
}

foo( 2 ); // 2 4 12
```

Hay tres ámbitos anidados inherentes en este ejemplo de código. Puede ser útil pensar en estos ámbitos como burbujas dentro de los demás.

![](/assets/Captura de pantalla 2017-08-27 a las 3.24.09 p.m..png)

Burbuja 1 abarca el ámbito global, y tiene sólo un identificador en ella: `foo`.

Burbuja 2 abarca el alcance de `foo`, que incluye los tres identificadores: `a`, `bar` y `b`.

Burbuja 3 abarca el alcance de `bar`, e incluye sólo un identificador: `c`.

Las burbujas de alcance se definen por donde se escriben los bloques de alcance, el cual está anidado uno dentro del otro, etc. En el capítulo siguiente, discutiremos diferentes unidades de alcance, pero _por ahora, supongamos que cada función crea una nueva Burbuja de alcance_.

La burbuja para `bar` está totalmente contenida dentro de la burbuja para `foo`, porque \(y sólo porque\) es donde elegimos definir las funciones de `bar`.

Observe que estas burbujas anidadas están anidadas estrictamente. No estamos hablando de diagramas de Venn donde las burbujas pueden cruzar los límites. En otras palabras, ninguna burbuja para alguna función puede existir simultáneamente \(parcialmente\) dentro de otras dos burbujas de alcance externo, así como ninguna función puede estar parcialmente dentro de cada una de las dos funciones principales.

### Consultas

La estructura y la ubicación relativa de estas burbujas de alcance explica completamente al Motor todos los lugares en los que necesita buscar para encontrar un identificador.

En el fragmento de código anterior, el Motor ejecuta la instrucción `console.log(..)` y busca las tres variables referenciadas `a`, `b` y `c`. Primero comienza con la burbuja más interna del ámbito, el ámbito de la `function bar(..)`. No encontrará una `a` allí, _por lo que sube un nivel_, a la próxima burbuja de ámbito más cercano, el ámbito de `foo(..)`. Encuentra `a` allí, por lo que usa esa `a`. Lo mismo para `b`. Pero `c`, se encuentra dentro de `bar(..)`.

Si hubiese habido `c` tanto dentro de `bar(..)` como dentro de `foo(...)`, la sentencia `console.log(..)` habría encontrado y usado el de `bar(..)`, sin llegar nunca a la de `foo`

**La búsqueda de alcance se detiene una vez que encuentra la primera coincidencia**. El mismo nombre de identificador se puede especificar en varias capas de ámbito anidado, lo cual se denomina "sombreado \(shadowing\)" \(el identificador interno "sombrea \(shadows\)" el identificador externo\). Independientemente del sombreado, la búsqueda del ámbito siempre comienza en el ámbito más interior del que se está ejecutando en ese momento, y va hacia el exterior/hacia-arriba hasta que encuentre la primera coincidencia y se detiene.

**Nota**: Las variables globales también son automáticamente propiedades del objeto global \(ventana en los navegadores, etc.\), por lo que es posible hacer referencia a una variable global no directamente por su nombre léxico, sino indirectamente como una referencia de propiedad del objeto global.

```js
window.a
```

Esta técnica da acceso a una variable global que de otro modo sería inaccesible debido a que se sombreaba. No obstante, no se puede acceder a variables no globales sombreadas.

No importa de dónde se invoque una función, ni siquiera cómo se invoca, su alcance léxico sólo se define por donde se declaró la función.

El proceso de búsqueda de alcance léxico sólo se aplica a identificadores de primera clase, como los `a`, `b` y `c`. Si tuviera una referencia a `foo.bar.baz` en un pedazo de código, la búsqueda de alcance léxico se aplicaría a encontrar el identificador `foo`, pero una vez que localiza esa variable, las reglas de acceso a la propiedad del objeto asumen el control para resolver `bar` y `baz`, respectivamente.







