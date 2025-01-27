### FUNCIONAMIENTO DEL MICROBIT 
#### Describe qué pasa en el punto 15 y cómo crees que esto se logre.
El microbit al estar enlazado a el pc envia señales que son controladas a partir de un codigo, en este caso las siguientes lineas controlan este punto
``` py
while True:
    if button_a.is_pressed():
        uart.write('A')
        sleep(500)
    if button_b.is_pressed():
        uart.write('B')
        sleep(500)
```
es un ciclo while infinito donde mediante un if se identifica el boton presionado y se "escribe" en pantalla la letra seleccionada y despues se da paso a un descanso en el cpu de medio segundo  
#### Describe qué pasa en el punto 16 y cómo crees que esto se logre.  
las lineas que controlan este punto son:  
``` py
if accelerometer.was_gesture('shake'):
        uart.write('C')
        sleep(500)
```
Es similar al punto anterior solo que en vez de controlar los botones presionados en el microbit, este controla la reaccion de un sensor del propio micro bit que reacciona al momento de sacudirlo, enviando la señal de que
la letra "c"  

#### Describe qué pasa en el punto 17 y cómo crees que esto se logre.  
las lineas que controlan este punto son:
``` py
if uart.any():
        data = uart.read(1)
        if data:
            if data[0] == ord('h'):
                display.show(Image.HEART)
                sleep(500)
                display.show(Image.HAPPY)
```
lo que pasa aqui es que mediante el uart el codigo envia señales a un dispositivo externo y analiza si el "data" es igual a "h", de ser asi envia una señal al microbit para que mediante sus leds muestre "imagenes" como 
en este cas el corazon y la cara feliz.

 
