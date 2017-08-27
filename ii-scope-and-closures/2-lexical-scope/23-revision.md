# 2.3 Revisión \(TL; DR\)

El alcance léxico significa que el alcance se define por las decisiones de tiempo de autor de donde se declaran las funciones. La fase de lexing de la compilación es esencialmente capaz de saber dónde y cómo se declaran todos los identificadores y así predecir cómo se buscarán durante la ejecución.

Dos mecanismos en JavaScript pueden "engañar" el ámbito léxico: `eval(..)` y `with`. El primero puede modificar el alcance léxico existente \(en tiempo de ejecución\) evaluando una cadena de "código" que tiene una o más declaraciones en él. Mientras que el último esencialmente crea un nuevo ámbito léxico \(de nuevo, en tiempo de ejecución\) tratando una referencia de objeto como un "ámbito" y las propiedades de ese objeto como identificadores de ámbito.

La desventaja de estos mecanismos es que derrota la capacidad del Motor para realizar optimizaciones en tiempo de compilación en cuanto a la búsqueda de scope, porque el Motor tiene que asumir pesimistamente que tales optimizaciones serán inválidas. El código se ejecutará más lentamente como resultado de usar cualquiera de las funciones. **No las use**.

