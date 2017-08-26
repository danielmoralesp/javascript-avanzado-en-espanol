# 1.2 Teoría del Compilador

Puede ser evidente por sí mismo, o puede ser sorprendente, dependiendo de su nivel de interacción con varios idiomas, pero a pesar de que JavaScript pertenece a la categoría general de lenguajes "dinámicos" o "interpretados", es de hecho un lenguaje compilado. No se compila con suficiente antelación, al igual que muchos lenguajes tradicionalmente compilados, ni tampoco los resultados de la compilación son portables entre varios sistemas distribuidos.

Pero, sin embargo, el motor de JavaScript realiza muchos de los mismos pasos, aunque de formas más sofisticadas de lo que comúnmente se sabe, que cualquier compilador de lenguaje tradicional.

En el proceso tradicional del lenguaje compilado, un fragmento del código fuente, su programa, experimentará típicamente tres pasos antes de que se ejecute, comúnmente llamado "compilación":

1. **Tokenizing / Lexing**: romper una cadena de caracteres en pedazos significativos \(al lenguaje\), llamados tokens. Por ejemplo, considere el programa: `var a = 2;`. Este programa probablemente se dividiría en los siguientes tokens: `var`, `a`, `=`, `2`, y `;`. Los espacios en blanco pueden o no ser persistidos como un símbolo, dependiendo de si es significativo o no.

   _Nota_: La diferencia entre tokenizing y lexing es sutil y académica, pero se centra en si estos símbolos se identifican de manera apátrida o con estado. En pocas palabras, si el tokenizer invocara reglas de análisis sintáctico para averiguar si `a` debería considerarse un token distinto o simplemente parte de otro token, eso sería lexing

2. **Parsing**: toma una corriente \(array\) de tokens y lo convierte en un árbol de elementos anidados, que colectivamente representan la estructura gramatical del programa. Este árbol se denomina "AST \(Abstract Syntax Tree\)" \(Árbol de sintaxis abstracto\).

   El árbol para `var a = 2`; Puede comenzar con un nodo de nivel superior llamado `VariableDeclaration`, con un nodo secundario llamado `Identifier` \(cuyo valor es `a`\), y otro llamado `AssignmentExpression` que tiene un hijo llamado `NumericLiteral` \(cuyo valor es `2`\).

3. **Code-Generation**: es el proceso de tomar un AST y convertirlo en código ejecutable. Esta parte varía mucho dependiendo del lenguaje, la plataforma a la que se está dirigiendo, etc.

   Por lo tanto, en lugar de quedar atascados en los detalles, sólo vamos a mano y decir que hay una manera de tomar nuestro AST descrito anteriormente para `var a = 2;` y convertirlo en un conjunto de instrucciones de máquina para crear realmente una variable llamada `a` \(incluida la reserva de memoria, etc.\), y luego almacenar un valor en `a`.

   Nota: Los detalles de cómo el motor gestiona los recursos del sistema son más profundos de lo que cavaremos, por lo que sólo daremos por sentado que el motor es capaz de crear y almacenar variables según sea necesario.



El motor JavaScript es mucho más complejo que estos tres pasos, al igual que la mayoría de los compiladores de otros lenguajes. Por ejemplo, en el proceso de análisis y generación de código, hay ciertamente pasos para optimizar el rendimiento de la ejecución, incluyendo el colapso de elementos redundantes, etc.

Por lo tanto, estoy pintando sólo con trazos amplios aquí. Pero creo que verá pronto por qué estos detalles que cubrimos, incluso a un nivel alto, son relevantes.

Por un lado, los motores de JavaScript no obtienen el lujo \(como otros compiladores de lenguaje\) de tener un montón de tiempo para optimizar, porque la compilación de JavaScript no ocurre en un paso de construcción con antelación, como si ocurre con otros lenguajes.

Para JavaScript, la compilación sucede, en muchos casos, en microsegundos \(o menos\) antes de ejecutar el código. Para garantizar un rendimiento más rápido, los motores JS utilizan todo tipo de trucos \(como JITs, compilación perezosa e incluso re-compilación en caliente, etc.\) que están mucho más allá del "alcance" de nuestra discusión aquí.

Digamos, por simplicidad, que cualquier fragmento de JavaScript tiene que ser compilado antes \(normalmente antes de que!\) se ejecute. Por lo tanto, el compilador JS tomará el programa `var a = 2;` y lo compila primero, y luego esta listo para ejecutarlo, normalmente de inmediato.

