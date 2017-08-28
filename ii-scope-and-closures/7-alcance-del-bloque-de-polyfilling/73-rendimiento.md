# 7.3 Rendimiento

Permítanme añadir una última nota rápida sobre el rendimiento de `try` / `catch`, y / o para abordar la pregunta, "¿por qué no sólo utilizar un IIFE para crear el scope?"

En primer lugar, el rendimiento de `try` / `catch` es más lento, pero no hay supuesto razonable de que tiene que ser de esa manera, o incluso que siempre será de esa manera. Dado que el transportista oficial de ES6 aprobado por TC39 utiliza `try` / `catch`, el equipo de Traceur ha pedido a Chrome que mejore el rendimiento de `try` / `catch`, y obviamente están motivados para hacerlo.

En segundo lugar, IIFE no es una comparación de manzanas a manzanas justa con `try` / `catch`, porque una función envuelta alrededor de cualquier código arbitrario cambia el significado, dentro de ese código, de esto, `return`, `break` y `continue`. IIFE no es un sustituto general adecuado. Sólo se puede utilizar manualmente en ciertos casos.

La pregunta realmente se convierte en: ¿quieres bloquear el ámbito, o no. Si lo hace, estas herramientas le proporcionan esa opción. Si no es así, sigue usando `var` y continúa tu codificación!

\[^note-traceur\]:[Google Traceur](http://traceur-compiler.googlecode.com/git/demo/repl.html)

\[^note-let\_er\]:[let-er](https://github.com/getify/let-er)

