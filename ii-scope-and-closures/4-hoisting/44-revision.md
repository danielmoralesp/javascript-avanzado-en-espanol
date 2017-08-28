# 4.4 Revisión \(TL; DR\)

Podemos ser tentados a mirar `var a = 2;` como una declaración, pero el Motor de JavaScript no lo ve así. Ve `var a` y `a = 2` como dos sentencias separadas, la primera es una tarea de fase-compilador y la segunda es una tarea de fase-ejecución.

Lo que esto lleva a que todas las declaraciones en un ámbito, independientemente de dónde aparezcan, se procesan primero antes de ejecutar el propio código. Se puede visualizar esto como declaraciones \(variables y funciones\) que se "mueven" a la parte superior de sus respectivos ámbitos, que llamamos "hoisting".

Las declaraciones mismas son elevadas, pero las asignaciones, incluso las asignaciones de las expresiones de la función, no se elevan.

Tenga cuidado con las declaraciones duplicadas, especialmente mezcladas entre las declaraciones normales de `var` y las declaraciones de funciones - ¡el peligro lo espera si lo hace!

