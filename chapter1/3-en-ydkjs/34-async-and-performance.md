# 3.4 Async & Performance

Los tres primeros títulos de esta serie se centran en la mecánica básica del lenguaje, pero el cuarto título se ramifica ligeramente para cubrir los patrones de la mecánica del lenguaje para gestionar la programación asincrónica. La asincronía no sólo es crítica para el rendimiento de nuestras aplicaciones, sino que se está convirtiendo cada vez más en un factor crítico en la capacidad de escritura y mantenimiento.

El libro comienza primero por aclarar una gran cantidad de terminología y confusión de conceptos en torno a cosas como "asíncrona", "paralela" y "concurrente", y explica en profundidad cómo estas cosas se aplican y no se aplican a JS.

Entonces nos movemos a examinar las devoluciones de llamada \(callbacks\) como el método primario de permitir la asincronía. Pero es aquí que vemos rápidamente que las callbacks por sí solas son irremediablemente insuficientes para las demandas modernas de la programación asíncrona. Identificamos dos deficiencias principales de las devoluciones de llamada: sólo la codificación: pérdida de confianza de inversión de control \(IoC\) y falta de capacidad lineal razonable .

Para abordar estas dos grandes deficiencias, ES6 introduce dos nuevos mecanismos \(y, de hecho, patrones\): las promesas y los generadores \(promises y generators\)

Las promesas son una envoltura independiente del tiempo alrededor de un "valor futuro", que le permite razonar y componer independientemente de si el valor está listo o no. Por otra parte, resuelven eficazmente las ediciones del trust de IoC encaminando devoluciones de llamada a través de un mecanismo confiables y componibles de la promesa.

Los generadores introducen un nuevo modo de ejecución para las funciones JS, con lo que el generador puede pausarse en los puntos de rendimiento y reanudarse asincrónicamente más tarde. La capacidad de pausa y reanudación permite que el código de búsqueda secuencial y secuencial en el generador se procese asincrónicamente detrás de escena. Haciendo esto, abordamos las confusiones de devoluciones de llamada no lineales, no locales y, por lo tanto, hacemos que nuestro código asincrónico sincronice el código para ser más razonable.

Pero es la combinación de promesas y generadores que "rinde \(yields\)" nuestro patrón de codificación asíncrono más efectivo hasta la fecha en JavaScript. De hecho, gran parte de la sofisticación futura de la asincronía que viene en ES7 y después se construirá seguramente sobre esta base. Para ser serio acerca de la programación eficaz en un mundo asíncrono, usted va a tener que sentirse muy cómodo con la combinación entre promesas y generadores.

Si las promesas y los generadores tienen que ver con la expresión de patrones que permiten que nuestros programas funcionen mejor al mismo tiempo y así lograr más procesamiento en un período más corto, JS tiene muchas otras facetas de optimización de rendimiento que vale la pena explorar.

El capítulo 5 profundiza en temas como el paralelismo de programas con los Workers Web y el paralelismo de datos con SIMD, así como técnicas de optimización de bajo nivel como ASM.js. El Capítulo 6 echa un vistazo a la optimización del rendimiento desde la perspectiva de técnicas de benchmarking apropiadas, incluyendo por qué tipo de rendimiento preocuparse y qué ignorar.

Escribir JavaScript significa efectivamente escribir código que puede romper las restricciones de estar ejecutado dinámicamente en una amplia gama de navegadores y otros entornos. Requiere una gran cantidad de intrincada y detallada planificación y esfuerzo de nuestras partes para llevar un programa de "funciona" a "funciona bien".

El título Async & Performance está diseñado para ofrecerte todas las herramientas y habilidades que necesitas para escribir código JavaScript razonable y eficaz.

