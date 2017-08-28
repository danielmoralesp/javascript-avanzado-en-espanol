# 7.1 Traceur

Google mantiene un proyecto llamado "Traceur" \[^ note-traceur\], que tiene la tarea de transponer las funciones de ES6 en pre-ES6 \(en su mayoría ES5, pero no todas!\) Para uso general. El comité TC39 se basa en esta herramienta \(y otros\) para probar la semántica de las características que especifican.

¿Qué produce Traceur a partir de nuestro fragmento? ¡Lo adivinaste!

```js
{
    try {
        throw undefined;
    } catch (a) {
        a = 2;
        console.log( a );
    }
}

console.log( a );
```

Por lo tanto, con el uso de estas herramientas, podemos empezar a aprovechar el ámbito del bloque sin importar si estamos apuntando a ES6 o no, porque `try` / `catch` ha estado alrededor \(y trabajado de esta manera\) desde los días de ES3.

