## Código para leer un carácter enviado por el puerto serial:
Este código permite al micro:bit recibir un carácter desde un puerto serial (como el monitor serial de Mu o el serial de Python) y mostrarlo en la pantalla LED.

```js
from microbit import *
import serial

# Establecer la conexión serial
uart = serial.Serial()

# Bucle principal
while True:
    if uart.any():  
        char_received = uart.read(1) 
        display.show(char_received)  
```
### Documentación del proceso:
Objetivo del experimento:

El objetivo es recibir un carácter desde el puerto serial y mostrarlo en la pantalla LED del micro:bit.
Hipótesis:

Al enviar un carácter desde un dispositivo (como un monitor serial), el micro:bit lo leerá y lo mostrará en su pantalla LED.
Proceso de experimentación:

Se configura el puerto serial en el micro:bit.
Se establece una conexión serial entre el micro:bit y el dispositivo que enviará los datos.
Se envían caracteres al micro:bit usando un programa Python o el Monitor Serial.
El micro:bit lee los caracteres recibidos a través del puerto serial y los muestra en su pantalla.
Resultados esperados:

Al enviar un carácter, el micro:bit debería mostrar dicho carácter en su pantalla LED.
Conclusiones:

El micro:bit es capaz de recibir datos a través del puerto serial y procesarlos para mostrar información en la pantalla. Este proceso es útil para la comunicación entre el micro:bit y otros dispositivos.
