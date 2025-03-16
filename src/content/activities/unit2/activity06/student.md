## Código para mostrar imágenes y texto en la pantalla LED del micro:bit:
```js
from microbit import *


display.show(Image.HAPPY)
sleep(1000)


display.show(Image.HEART)
sleep(1000)


sun_image = Image("09090:90909:09090:00900:00000")
display.show(sun_image)
sleep(1000)


display.scroll("Hola")
``` 
### Explicación del funcionamiento:  
display.show(): Muestra imágenes o texto en la pantalla del micro:bit. Aquí se muestran una carita feliz, un corazón y una imagen personalizada de un sol.

Image: Se utilizan imágenes predefinidas (Image.HAPPY, Image.HEART) y una personalizada creada con una cadena de números que representa la pantalla LED.

sleep(): Pausa la ejecución del código durante el tiempo especificado (1 segundo en este caso).

display.scroll(): Muestra el texto "Hola" desplazándose de izquierda a derecha en la pantalla.
