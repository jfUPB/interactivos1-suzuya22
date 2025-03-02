## Entradas y salidas del micro:bit:
### Entradas:

Botones A y B  
Los botones A y B permiten que el micro:bit reciba entradas físicas al ser presionados. Se utilizan comúnmente para interactuar con el usuario.  
Ejemplo de uso:  
Detectar si se ha presionado el botón A o B.  
Sensor de temperatura  
El micro:bit puede medir la temperatura del entorno mediante un sensor incorporado.  
Ejemplo de uso:  
Leer la temperatura ambiental.  
Sensor de luz (LDR)  
El micro:bit tiene un sensor de luz que puede detectar la intensidad de la luz ambiental.  
Ejemplo de uso:  
Controlar dispositivos en función de la luz disponible.  
Pin de entrada (P0, P1, P2)  
Los pines P0, P1 y P2 pueden funcionar como entradas digitales o analógicas. Permiten conectar dispositivos como sensores externos.  
Ejemplo de uso:  
Leer datos de un sensor de humedad conectado a un pin.  

### Salidas:  

Pantalla LED (5x5)  
El micro:bit tiene una pantalla de 5x5 LEDs que se puede utilizar para mostrar caracteres, patrones o gráficos.  
Ejemplo de uso:  

Mostrar un mensaje en la pantalla cuando un botón se presiona.  
Motor de vibración  
El micro:bit tiene un motor de vibración que se puede activar mediante código, y se usa comúnmente como una señal táctil.  
Ejemplo de uso:  

Generar una vibración cuando un evento o acción ocurre.  
Pines de salida (P0, P1, P2)  
Los pines también se pueden configurar como salidas digitales o analógicas para enviar señales o controlar dispositivos externos.  
Ejemplo de uso:  

Encender un LED o controlar un motor conectado a un pin de salida.  
Buzzer (altavoz) conectado a un pin  
Puedes conectar un buzzer a uno de los pines del micro:bit para producir sonidos o alertas.  
Ejemplo de uso:  

Emitir un sonido cuando un evento sucede.  

## Funciones de Micropython:
### Funciones para entradas:
1. button_a.is_pressed()
Esta función devuelve True si el botón A está presionado y False si no lo está.
Ejemplo de código:
```js
from microbit import *
while True:
    if button_a.is_pressed():
        display.show('A')
    else:
        display.clear()
```

2. temperature()
Esta función devuelve la temperatura medida por el sensor del micro:bit en grados Celsius.
Ejemplo de código:
```js
from microbit import *
while True:
    temp = temperature()
    display.scroll(str(temp))
    sleep(1000)
```
3. pin0.read_analog()
Esta función lee un valor analógico del pin 0 (P0), el cual puede estar conectado a sensores como un LDR o un sensor de temperatura externo.
Ejemplo de código:
```js
from microbit import *
while True:
    sensor_value = pin0.read_analog()
    display.scroll(str(sensor_value))
    sleep(1000)
```


### Funciones para salidas:

1. display.show()
Muestra un mensaje o patrón de LEDs en la pantalla del micro:bit.
Ejemplo de código:
```js
from microbit import *
display.show('Hello')
```
2. pin0.write_digital()
Esta función permite enviar una señal digital de 0 o 1 a un pin de salida, controlando dispositivos como LEDs.
Ejemplo de código:
```js
from microbit import *
while True:
    pin0.write_digital(1)  # Enciende el LED
    sleep(1000)
    pin0.write_digital(0)  # Apaga el LED
    sleep(1000)
```
3. pin1.write_analog()
Permite enviar una señal analógica (PWM) a un pin de salida. Esto puede ser útil para controlar la velocidad de un motor o el brillo de un LED.
Ejemplo de código:

```js
from microbit import *
while True:
    pin1.write_analog(512)  # Señal analógica media
    sleep(1000)
    pin1.write_analog(1023)  # Señal analógica máxima
    sleep(1000)
```
