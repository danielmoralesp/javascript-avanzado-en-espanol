# 3.1 This & Object Prototypes

Tal vez uno de los más difundidos y persistentes mistruths sobre JavaScript es que la palabra clave `this` se refiere a la función en la que aparece. Grave error.

La palabra clave `this` se enlaza dinámicamente en función de cómo se ejecuta la función en cuestión y resulta que hay cuatro reglas simples para comprender y determinar completamente la vinculación `this`.

Muy relacionado con la palabra clave `this` está el mecanismo de prototipo de objeto, que es una cadena de búsqueda de propiedades, similar a cómo se encuentran las variables de alcance de léxico. Pero envuelto en los prototipos es el otro gran error sobre JS: la idea de emular \(fake\) clases y \(lo que se llama "prototipo"\) la herencia.

Desafortunadamente, el deseo de traer el pensamiento de patrones de diseño de clase y herencia a JavaScript es casi lo peor que puedes intentar hacer, porque mientras la sintaxis te engañe pensando que hay algo como clases presentes, de hecho el mecanismo prototipo es fundamentalmente opuesto en su comportamiento.

Lo que está en cuestión es si es mejor ignorar el desajuste y pretender que lo que está implementando es "herencia", o si es más apropiado aprender y abrazar cómo funciona realmente el prototipo del sistema. Esta última es más apropiadamente llamada "delegación de comportamiento".

Esto es más que una preferencia sintáctica. **La delegación es un patrón de diseño totalmente diferente, y más potente, que reemplaza la necesidad de diseñar con clases y herencia**. Sin embargo, estas afirmaciones vuelan absolutamente en la cara de casi todos los demás blog post, libros y conferencias que hablan sobre el tema de la totalidad de la vida de JavaScript.

Las afirmaciones que hago con respecto a la delegación versus la herencia vienen no de una aversión al lenguaje y su sintaxis, sino del deseo de ver la verdadera capacidad del lenguaje adecuadamente apalancado y la interminable confusión y frustración por fin borrada.

Pero el caso que hago con respecto a los prototipos y la delegación es mucho más complicado que lo que voy a disfrutar aquí. Si estás listo para reconsiderar todo lo que piensas que sabes sobre las "clases" de JavaScript y la "herencia", te ofrezco la oportunidad de "tomar la píldora roja" \(Matrix 1999\) y echa un vistazo a los capítulos 4-6 del Título This &  de los prototipos de esta serie.



