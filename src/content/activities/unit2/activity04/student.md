### Experimento 1: Comprobación de los botones A y B

#### ¿Qué querías comprobar?
Quiero comprobar si el micro:bit detecta correctamente cuándo se presionan los botones A y B.

#### ¿Cuál fue tu hipótesis?
Cuando presione el botón A, el micro:bit debería mostrar un "A" en la pantalla, y cuando presione el botón B, debería mostrar una "B". Si no se presionan, la pantalla debería estar vacía.

Código involucrado en el experimento:
```js
from microbit import *

while True:
    if button_a.is_pressed():
        display.show('A')
    elif button_b.is_pressed():
        display.show('B')
    else:
        display.clear()
```
#### Descripción de los resultados:
Al presionar el botón A, la pantalla mostró la letra "A". Al presionar el botón B, mostró la letra "B". Cuando no presioné ninguno de los botones, la pantalla se apagó.

#### Análisis de esos resultados:
Esto demuestra que los botones A y B están funcionando como entradas digitales. Cuando se presionan, el micro:bit detecta el cambio en el estado del botón, lo que permite mostrar un valor o realizar una acción.

#### Conclusiones:
Los botones A y B del micro:bit funcionan correctamente como entradas digitales, y el código responde correctamente a sus interacciones.


### Experimento 2: Comprobación del botón virtual del logo
#### ¿Qué querías comprobar?
Quiero comprobar si el micro:bit detecta correctamente cuando se toca el botón virtual del logo (el centro de la placa).

#### ¿Cuál fue tu hipótesis?
Cuando toque el logo del micro:bit, se debería activar una acción, como mostrar un "L" en la pantalla.

Código involucrado en el experimento:
```js
from microbit import *

while True:
    if logo.is_pressed():
        display.show('L')
    else:
        display.clear()
```
#### Descripción de los resultados:
Al tocar el logo, la pantalla mostró la letra "L". Cuando dejé de tocarlo, la pantalla se apagó.

#### Análisis de esos resultados:
El botón virtual del logo funciona como una entrada digital, similar a los botones A y B. Detecta el toque de la mano al presionar el centro de la placa, lo que permite activar una respuesta visual.

#### Conclusiones:
El botón virtual del logo también funciona correctamente como una entrada digital, y el micro:bit responde adecuadamente al contacto físico con la placa.

### Experimento 3: Comprobación de múltiples entradas simultáneas
#### ¿Qué querías comprobar?
Quiero comprobar cómo reacciona el micro:bit cuando presiono los tres botones (A, B y el logo) simultáneamente.

#### ¿Cuál fue tu hipótesis?
Cuando presione simultáneamente los botones A, B y el logo, el micro:bit debería registrar cada entrada por separado, pero en este caso el código mostrará solo el valor de uno de ellos, ya que solo se puede mostrar un valor a la vez.

Código involucrado en el experimento:
```js
from microbit import *

while True:
    if button_a.is_pressed():
        display.show('A')
    elif button_b.is_pressed():
        display.show('B')
    elif logo.is_pressed():
        display.show('L')
    else:
        display.clear()
````
#### Descripción de los resultados:
Cuando presioné varios botones al mismo tiempo, el micro:bit siempre mostró solo la letra correspondiente al primer botón presionado, ya que el código está configurado para mostrar solo uno a la vez. Si presioné A primero, mostró "A", si presioné B primero, mostró "B", y así sucesivamente.

#### Análisis de esos resultados:
Este comportamiento ocurre porque el código está diseñado con una estructura elif, lo que significa que el micro:bit solo ejecutará el primer bloque que sea verdadero. No tiene la capacidad de mostrar múltiples valores a la vez debido a la limitación de la pantalla y la lógica del código.

#### Conclusiones:
El micro:bit solo responde a una entrada a la vez si están presionados varios botones, y se mostrará solo el valor correspondiente al primer botón presionado debido a cómo se organizó el código.
