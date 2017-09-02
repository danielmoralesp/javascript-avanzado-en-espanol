# 2.3 Todo en orden

Por lo tanto, ahora hemos descubierto las 4 reglas para vincular `this` en las llamadas de función. Todo lo que necesita hacer es encontrar el sitio de llamada e inspeccionarlo para ver qué regla se aplica. Pero, ¿qué pasa si el sitio de llamada tiene múltiples reglas elegibles? Debe haber un orden de precedencia a estas reglas, por lo que demostraremos a continuación cuál es el orden para aplicar las reglas.

Debe quedar claro que la vinculación por defecto es la regla de prioridad más baja de las 4. Por lo tanto, vamos a dejarla de lado.

¿Cuál es más precedente, vinculación implícita o vinculación explícita? Vamos a probarlo:

```js
function foo() {
    console.log( this.a );
}

var obj1 = {
    a: 2,
    foo: foo
};

var obj2 = {
    a: 3,
    foo: foo
};

obj1.foo(); // 2
obj2.foo(); // 3

obj1.foo.call( obj2 ); // 3
obj2.foo.call( obj1 ); // 2
```

Por lo tanto, la vinculación explicita tiene prioridad sobre la vinculacion implicita, lo que significa que debe preguntar primero si se aplica la vinculación explicita antes de comprobar la vinculación implícita.

Ahora, sólo necesitamos averiguar dónde se ajusta el nuevo vinculo en la precedencia.

```js
function foo(something) {
    this.a = something;
}

var obj1 = {
    foo: foo
};

var obj2 = {};

obj1.foo( 2 );
console.log( obj1.a ); // 2

obj1.foo.call( obj2, 3 );
console.log( obj2.a ); // 3

var bar = new obj1.foo( 4 );
console.log( obj1.a ); // 2
console.log( bar.a ); // 4
```

OK, la nueva vinculación es más precedente que la vinculación implícita. Pero ¿crees que la nueva vinculación es más o menos precedente que la vinculación explícita?

**Nota**: `new` y `call` / `apply` no se pueden usar juntos, por lo que no se permite `new foo.call(obj1)`, para probar el nuevo vinculo directamente contra el vinculo explícito. Pero todavía podemos usar una vinculación dura \(hard binding\) para probar la precedencia de las dos reglas.

Antes de explorar eso en un listado de código, piense en cómo funciona físicamente el vinculo duro, que es `Function.prototype.bind(..)` crea una nueva función de encapsulamiento que está codificada para ignorar su propia vinculación \(lo que sea\), y utilizar un manual que proporcionamos.

Por ese razonamiento, parecería obvio asumir que la vinculación dura \(que es una forma de vinculación explícita\) es más precedente que una nueva vinculación, y por lo tanto no puede ser reemplazada por una nueva.

Vamos a revisar:

```js
function foo(something) {
    this.a = something;
}

var obj1 = {};

var bar = foo.bind( obj1 );
bar( 2 );
console.log( obj1.a ); // 2

var baz = new bar( 3 );
console.log( obj1.a ); // 2
console.log( baz.a ); // 3
```

Whoa! `bar` está limitado contra `obj1`, pero `new bar(3)` no cambió `obj1.a` para ser `3` como habríamos esperado. En su lugar, la llamada a `bar(..)` con límite rígido \(a `obj1`\) puede ser reemplazada por `new`. Desde que se aplicó `new`, obtuvimos el objeto recién creado, que denominamos `baz`, y vemos de hecho que `baz.a` tiene el valor `3`.

Esto debería ser sorprendente si vuelves a nuestro "falso" `bind` helper:

```js
function bind(fn, obj) {
    return function() {
        fn.apply( obj, arguments );
    };
}
```

Si razonas sobre cómo funciona el código del ayudante, no tiene una forma para que una llamada `new` de operador anule la vinculación de hard-obj como acabamos de observar.

Pero el built-in `Function.prototype.bind(...)` como ES5 es más sofisticado, un poco así, de hecho. Aquí está el polyfill \(ligeramente reformateado\) proporcionado por la página MDN para `bind(..)`:

```js
if (!Function.prototype.bind) {
    Function.prototype.bind = function(oThis) {
        if (typeof this !== "function") {
            // closest thing possible to the ECMAScript 5
            // internal IsCallable function
            throw new TypeError( "Function.prototype.bind - what " +
                "is trying to be bound is not callable"
            );
        }

        var aArgs = Array.prototype.slice.call( arguments, 1 ),
            fToBind = this,
            fNOP = function(){},
            fBound = function(){
                return fToBind.apply(
                    (
                        this instanceof fNOP &&
                        oThis ? this : oThis
                    ),
                    aArgs.concat( Array.prototype.slice.call( arguments ) )
                );
            }
        ;

        fNOP.prototype = this.prototype;
        fBound.prototype = new fNOP();

        return fBound;
    };
}
```

Nota: El `bind()` polyfill mostrado anteriormente difiere del built-in `bind(..)` en ES5 con respecto a las funciones enlazadas que se usarán con `new` \(vea más adelante por qué es útil\). Debido a que el polyfill no puede crear una función sin un `.prototype`como lo hace la utilidad incorporada, hay alguna indirección matizada para aproximar el mismo comportamiento. Pise con cuidado si va a utilizar el `new` con una función de hard-bound y confía en este polyfill.

La parte que está permitiendo el `new` overriding es:

```js
this instanceof fNOP &&
oThis ? this : oThis

// ... and:

fNOP.prototype = this.prototype;
fBound.prototype = new fNOP();
```

vamos aqui 2 de septiembre 2017

