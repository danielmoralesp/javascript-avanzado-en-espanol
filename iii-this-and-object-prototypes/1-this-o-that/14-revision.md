# 1.4 Revisión \(TL; DR\)

El enlace `this` es una fuente constante de confusión para el desarrollador de JavaScript que no se toma el tiempo para aprender cómo funciona realmente el mecanismo. Las conjeturas, el ensayo y el error y el copia y pegue de las respuestas de StackOverflow no son una forma efectiva o adecuada de aprovechar este importante mecanismo.

Para aprender `this`, primero tiene que aprender lo que no es `this`, a pesar de cualquier suposición o concepto erróneo que pueden llevarlo por esos caminos. `this` no es ni una referencia a la función en sí, ni una referencia al ámbito léxico de la función.

`this` es en realidad un enlace que se realiza cuando se invoca una función y lo que hace referencia se determina totalmente por el sitio de llamada donde se llama la función.

