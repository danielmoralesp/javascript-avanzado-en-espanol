# 5.5 Módulos

Existen otros patrones de código que aprovechan la potencia de los closures, pero que no parecen estar sobre la superficie de los callbacks. Examinemos el más poderoso de ellos: el módulo.

```js
function foo() {
	var something = "cool";
	var another = [1, 2, 3];

	function doSomething() {
		console.log( something );
	}

	function doAnother() {
		console.log( another.join( " ! " ) );
	}
}
```

Como este código se encuentra en este momento, no hay un closure observable en curso. Simplemente tenemos algunas variables de datos privadas `something` y `another`, y un par de funciones internas `doSomething()` y `doAnother()`, que tienen alcance lexical \(y, por tanto, closure!\) Sobre el ámbito interno de `foo()`.

Pero ahora considere:

```js
function CoolModule() {
	var something = "cool";
	var another = [1, 2, 3];

	function doSomething() {
		console.log( something );
	}

	function doAnother() {
		console.log( another.join( " ! " ) );
	}

	return {
		doSomething: doSomething,
		doAnother: doAnother
	};
}

var foo = CoolModule();

foo.doSomething(); // cool
foo.doAnother(); // 1 ! 2 ! 3
```

Este es el patrón en JavaScript que llamamos módulo. La forma más común de implementar el patrón de módulo es a menudo llamado "Módulo Revelador \(Revealing Module\)", y es la variación que presentamos aquí.

Vamos a examinar algunas cosas acerca de este código.

En primer lugar, `CoolModule()` es sólo una función, pero tiene que ser invocado para que haya una instancia de módulo creada. Sin la ejecución de la función externa, la creación del ámbito interno y los closures no se producirían.

En segundo lugar, la función `CoolModule()` devuelve un objeto, denotado por la sintaxis de objeto-literal `{key: value, ...}`. El objeto que devolvemos tiene referencias sobre él a nuestras funciones internas, pero no a nuestras variables de datos internas. Los mantenemos ocultos y privados. Es apropiado pensar en este valor de retorno de objeto como esencialmente una API pública para nuestro módulo.

Este valor de retorno del objeto se asigna en última instancia a la variable externa `foo`, y luego podemos acceder a los métodos de propiedad en la API, como `foo.doSomething()`.

**Nota**: No se requiere que devolvamos un objeto real \(literal\) de nuestro módulo. Podríamos regresar una función interna directamente. JQuery es en realidad un buen ejemplo de esto. Los identificadores `jQuery` y `$` son la API pública para el jQuery "module", pero son, ellos mismos, sólo una función \(que puede tener propiedades, ya que todas las funciones son objetos\).

Las funciones `doSomething()` y `doAnother()` tienen closures sobre el ámbito interno del módulo "instance" \(llegado al invocar `CoolModule()`\). Cuando transportamos esas funciones fuera del alcance léxico, a través de las referencias de propiedad sobre el objeto que devolvemos, hemos establecido ahora una condición por la cual el closure puede ser observado y ejercido.

Para decirlo más sencillamente, hay dos "requisitos" para que el patrón de módulo sea ejercido:

1. Debe haber una función envolvente externa, y debe invocarse al menos una vez \(cada vez crea una instancia de módulo nuevo\).
2. La función de inclusión debe devolver al menos una función interna, por lo que esta función interna tiene un closure sobre el ámbito privado, y puede acceder y/o modificar ese estado privado.

Un objeto con una propiedad de función en él solo no es realmente un módulo. Un objeto que es devuelto de una invocación de función que sólo tiene propiedades de datos en él y no hay funciones cerradas no es realmente un módulo, en el sentido observable.

El fragmento de código anterior muestra un creador de módulos autónomo denominado `CoolModule()` que puede invocarse cualquier número de veces, cada vez que se crea una nueva instancia de módulo. Una ligera variación en este patrón es cuando sólo te interesa tener una instancia, un "singleton" de clases:

```js
var foo = (function CoolModule() {
	var something = "cool";
	var another = [1, 2, 3];

	function doSomething() {
		console.log( something );
	}

	function doAnother() {
		console.log( another.join( " ! " ) );
	}

	return {
		doSomething: doSomething,
		doAnother: doAnother
	};
})();

foo.doSomething(); // cool
foo.doAnother(); // 1 ! 2 ! 3
```

Aquí, convirtió nuestra función de módulo en un IIFE \(véase el capítulo 3\), e inmediatamente lo invocó y asignó su valor de retorno directamente a nuestro módulo único identificador de instancia `foo`.

Los módulos son sólo funciones, por lo que pueden recibir parámetros:

```ruby
function CoolModule(id) {
	function identify() {
		console.log( id );
	}

	return {
		identify: identify
	};
}

var foo1 = CoolModule( "foo 1" );
var foo2 = CoolModule( "foo 2" );

foo1.identify(); // "foo 1"
foo2.identify(); // "foo 2"
```

Otra variación ligera pero potente en el patrón de módulo es nombrar el objeto que estás regresando como tu API pública:

```js
var foo = (function CoolModule(id) {
	function change() {
		// modifying the public API
		publicAPI.identify = identify2;
	}

	function identify1() {
		console.log( id );
	}

	function identify2() {
		console.log( id.toUpperCase() );
	}

	var publicAPI = {
		change: change,
		identify: identify1
	};

	return publicAPI;
})( "foo module" );

foo.identify(); // foo module
foo.change();
foo.identify(); // FOO MODULE
```

Al conservar una referencia interna al objeto API público dentro de la instancia del módulo, puede modificar la instancia del módulo desde el interior, incluida la adición y eliminación de métodos, propiedades y el cambio de sus valores.

### Módulos modernos

Varios cargadores/administradores de dependencias de módulos terminan esencialmente con este patrón de definición de módulos en una API amigable. En lugar de examinar cualquier biblioteca en particular, permítanme presentar una prueba de concepto muy simple para fines \(sólo\) de ilustración:

```js
var MyModules = (function Manager() {
	var modules = {};

	function define(name, deps, impl) {
		for (var i=0; i<deps.length; i++) {
			deps[i] = modules[deps[i]];
		}
		modules[name] = impl.apply( impl, deps );
	}

	function get(name) {
		return modules[name];
	}

	return {
		define: define,
		get: get
	};
})();
```

La parte clave de este código es `modules[name] = impl.apply(impl, deps)`. Esto invoca la función de contenedor de definición de un módulo \(pasando en cualquier dependencia\) y almacena el valor de retorno, la API del módulo, en una lista interna de módulos rastreados por nombre.

Y aquí es cómo podría utilizarlo para definir algunos módulos:

```js
MyModules.define( "bar", [], function(){
	function hello(who) {
		return "Let me introduce: " + who;
	}

	return {
		hello: hello
	};
} );

MyModules.define( "foo", ["bar"], function(bar){
	var hungry = "hippo";

	function awesome() {
		console.log( bar.hello( hungry ).toUpperCase() );
	}

	return {
		awesome: awesome
	};
} );

var bar = MyModules.get( "bar" );
var foo = MyModules.get( "foo" );

console.log(
	bar.hello( "hippo" )
); // Let me introduce: hippo

foo.awesome(); // LET ME INTRODUCE: HIPPO
```

Los módulos "`foo`" y "`bar`" se definen con una función que devuelve una API pública. "`foo`" incluso recibe la instancia de "`bar`" como un parámetro de dependencia, y puede usarlo en consecuencia.

Pasa algún tiempo examinando estos fragmentos de código para comprender completamente el poder de los closures puestos a usar para nuestros propios propósitos. La clave para llevar es que no hay realmente ninguna "magia" particular para los administradores de módulos. Reúnen ambas características del patrón de módulo I enumerado anteriormente: invocando un contenedor de definición de función y manteniendo su valor de retorno como API para ese módulo.

En otras palabras, los módulos son sólo módulos, incluso si usted pone una herramienta de envoltura amigable encima de ellos.

### Módulos Futuros

ES6 agrega soporte de sintaxis de primera clase para el concepto de módulos. Cuando se carga a través del sistema de módulos, ES6 trata un archivo como un módulo separado. Cada módulo puede importar otros módulos o miembros específicos de la API, así como exportar sus propios miembros API públicos.

**Nota**: Los módulos basados ​​en funciones no son un patrón estáticamente reconocido \(algo que el compilador conoce\), por lo que su API semántica no se consideran hasta el momento de la ejecución. Es decir, puede modificar realmente la API de un módulo durante el tiempo de ejecución \(véase la anterior discusión publicAPI\).

Por el contrario, las API del módulo ES6 son estáticas \(las API no cambian en tiempo de ejecución\). Dado que el compilador lo sabe, puede \(y lo hace!\) comprobar durante la compilación \(carga de archivos y\) que existe una referencia a un miembro de la API de un módulo importado. Si no existe la referencia de API, el compilador produce un error "anticipado" en tiempo de compilación, en lugar de esperar la resolución dinámica en tiempo de ejecución tradicional \(y los errores, si los hay\).

Los módulos ES6 no tienen un formato "en línea", sino que deben definirse en archivos separados \(uno por módulo\). Los navegadores / motores tienen un "cargador de módulos" por defecto \(que es sobre-escribible, pero eso está bien más allá de nuestra discusión aquí\) que sincronizadamente carga un archivo de módulo cuando se importa.

Considere:

**bar.js**

```js
function hello(who) {
	return "Let me introduce: " + who;
}

export hello;
```

**foo.js**

```js
// import only `hello()` from the "bar" module
import hello from "bar";

var hungry = "hippo";

function awesome() {
	console.log(
		hello( hungry ).toUpperCase()
	);
}

export awesome;
```

```js
// import the entire "foo" and "bar" modules
module foo from "foo";
module bar from "bar";

console.log(
	bar.hello( "rhino" )
); // Let me introduce: rhino

foo.awesome(); // LET ME INTRODUCE: HIPPO
```

**Nota**: Deberían crearse archivos separados "`foo.js`" y "`bar.js`", con el contenido como se muestra en los dos primeros fragmentos, respectivamente. A continuación, su programa cargará / importará esos módulos para usarlos, como se muestra en el tercer fragmento.

`import` importa uno o más miembros de la API de un módulo en el ámbito actual, cada uno a una variable enlazada \(`hello` en nuestro caso\). `module` importa una API de módulo completa a una variable enlazada \(`foo`, `bar` en nuestro caso\). `export` exporta un identificador \(variable, función\) a la API pública para el módulo actual. Estos operadores pueden utilizarse tantas veces como sea necesario en la definición de un módulo.

El contenido dentro del archivo de módulo se trata como si estuviera encerrado en un closure de ámbito, al igual que con los módulos de función de closures vistos anteriormente.

