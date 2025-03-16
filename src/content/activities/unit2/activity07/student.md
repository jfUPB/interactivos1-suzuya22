"" Código para generar sonidos con el micro:bit:
```js
from microbit import *
import music


music.play(music.PRELUDE)
sleep(1000)


pitch(500, 500)  # 500 Hz durante 500 ms
sleep(1000)
```
### Explicación del funcionamiento:
music.play(): La función play de la librería music permite reproducir melodías predefinidas. En este caso, estamos utilizando music.PRELUDE, que es una secuencia musical corta y sencilla que se reproduce en el micro:bit. El sleep(1000) pausa la ejecución durante un segundo para que el sonido termine de sonar.

music.pitch(): Esta función permite generar un tono personalizado especificando la frecuencia (en hertzios) y la duración (en milisegundos). En el código, usamos pitch(500, 500), lo que genera un tono de 500 Hz que dura medio segundo.

sleep(): En ambos casos, el sleep se utiliza para controlar la duración de la pausa entre sonidos, asegurando que no se solapen o se corten abruptamente.
