# 1.1 Código

Empecemos desde el principio.

Un programa, a menudo denominado código fuente o código, es un conjunto de instrucciones especiales para indicar al equipo qué tareas realizar. Normalmente, el código se guarda en un archivo de texto, aunque con JavaScript también se puede escribir código directamente en una consola de desarrollador en un navegador, que veremos en breve.

Las reglas para el formato válido y las combinaciones de instrucciones se llaman un lenguaje informático, a veces se refiere como su sintaxis, lo mismo que el inglés te dice cómo deletrear palabras y cómo crear oraciones válidas usando palabras y signos de puntuación.

### Declaraciones

En un lenguaje de computadora, un grupo de palabras, números y operadores que realiza una tarea específica es una sentencia/declaración. En JavaScript, una declaración podría verse como sigue:

```js
a = b * 2;
```

Los caracteres `a` y `b` se llaman variables \(vea "Variables"\), que son como simples cajas en las que puede almacenar cualquiera de sus cosas. En los programas, las variables contienen valores \(como el número 42\) que debe usar el programa. Piense en ellos como marcadores de posición simbólicos para los valores mismos.

Por el contrario, el 2 es sólo un valor en sí mismo, llamado un valor literal, porque está solo sin ser almacenado en una variable.

Los caracteres `=` y `*` son operadores \(ver "Operadores"\) - realizan acciones con valores y variables tales como asignación y multiplicación matemática.

La mayoría de las declaraciones en JavaScript terminan con un punto y coma \(`;`\) al final.

La afirmación `a = b * 2`; Le dice a la computadora, aproximadamente, que obtenga el valor actual almacenado en la variable `b`, multiplique ese valor por 2, y luego guarde el resultado en otra variable a la que llamamos `a`.

Los programas son sólo colecciones de muchas declaraciones de este tipo, que en conjunto describen todos los pasos que se requieren para realizar el propósito de su programa.

### Expresiones

Las declaraciones se componen de una o más expresiones. Una expresión es cualquier referencia a una variable o valor, o un conjunto de variables y valores combinados con operadores.

Por ejemplo:

```js
a = b * 2;
```

Esta declaración tiene cuatro expresiones en ella:

2 es una expresión de valor literal

b es una expresión variable, lo que significa recuperar su valor actual

b \* 2 es una expresión aritmética, que significa hacer la multiplicación

a = b \* 2 es una expresión de asignación, lo que significa asignar el resultado de la expresión b \* 2 a la variable a \(sobre las asignaciones posteriores\)

Una expresión general que se mantiene sola también se denomina declaración de expresión, como la siguiente:

```js
b * 2;
```

Esta declaración de una expresión no es muy común o útil, ya que en general no tendría ningún efecto en el funcionamiento del programa: recuperaría el valor de b y lo multiplicaría por 2, pero luego no haría nada con ese resultado.

Una sentencia de expresión más común es una instrucción de expresión de llamada \(véase "Funciones"\), ya que la sentencia completa es la expresión de llamada de la función en sí:

```js
alert( a );
```

### Ejecutar un programa

¿De qué manera esas colecciones de declaraciones de programación le dicen a la computadora qué hacer? El programa necesita ser ejecutado, también conocido como ejecutar el programa.

Declaraciones como `a = b * 2` son útiles para los desarrolladores al leer y escribir, pero en realidad no están en una forma en que la computadora pueda entender directamente. Así que una utilidad especial en la computadora \(ya sea un intérprete o un compilador\) se utiliza para traducir el código que escribe en comandos que una computadora puede entender.

Para algunos lenguajes de computadora, esta traducción de los comandos se hace típicamente de arriba a abajo, línea por línea, cada vez que se ejecuta el programa, se llama generalmente interpretar el código.

Para otros idiomas, la traducción se hace antes de tiempo, llamada compilación del código, por lo que cuando el programa se ejecuta más tarde, lo que está en ejecución es en realidad las instrucciones de la computadora ya compilada listo para ejecutarse.

Normalmente, se afirma que JavaScript se interpreta, porque el código fuente de JavaScript se procesa cada vez que se ejecuta. Pero eso no es del todo exacto. El motor de JavaScript en realidad compila el programa sobre la marcha y luego ejecuta inmediatamente el código compilado.

Nota: Para obtener más información sobre la compilación de JavaScript, consulte los dos primeros capítulos del título Scope & Closures de esta serie.





