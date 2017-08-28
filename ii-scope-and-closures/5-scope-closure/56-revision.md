# 5.6 Revisión \(TL; DR\)

El closure parece a los no ilustrados como un mundo místico separado dentro de JavaScript que sólo las pocas almas más valientes pueden alcanzar. Pero en realidad es sólo un hecho estándar y casi obvio de cómo escribimos código en un entorno de ámbito léxico, donde las funciones son valores y se pueden transmitir a voluntad.

**El closure es cuando una función puede recordar y acceder a su alcance léxico incluso cuando se invoca fuera de su alcance léxico**.

Los closure pueden hacernos tropezar, por ejemplo con bucles, si no tenemos cuidado de reconocerlos y cómo funcionan. Pero también son una herramienta inmensamente poderosa, permitiendo patrones como módulos en sus diversas formas.

Los módulos requieren dos características clave: 1\) invocación de una función de envoltura externa, para crear el ámbito de inclusión y 2\) el valor de retorno de la función de envoltura debe incluir referencia a al menos una función interna que tiene closure sobre el ámbito interno privado del envoltorio.

Ahora podemos ver closures alrededor de nuestro código existente, y tenemos la capacidad de reconocer y aprovecharlos para nuestro propio beneficio!

