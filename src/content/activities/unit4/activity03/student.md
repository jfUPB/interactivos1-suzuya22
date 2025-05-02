## Caso de estudio: Comunicación de micro:bit con p5.js vía UART
 Exploración y análisis del código del micro:bit
 Código analizado
python
Copiar
Editar
from microbit import *

uart.init(115200)
display.set_pixel(0,0,9)

while True:
    xValue = accelerometer.get_x()
    yValue = accelerometer.get_y()
    aState = button_a.is_pressed()
    bState = button_b.is_pressed()
    data = "{},{},{},{}\n".format(xValue, yValue, aState, bState)
    uart.write(data)
    sleep(100) # Envia datos a 10 Hz
## ¿Qué entendí del código?
Yo entendí que este código establece una comunicación serial (UART) entre el micro:bit y un computador o dispositivo externo, con una velocidad de 115200 baudios. El micro:bit, cada 100 milisegundos, envía por el puerto serial los siguientes datos:

El valor del eje X del acelerómetro (xValue),

El valor del eje Y del acelerómetro (yValue),

El estado del botón A (aState),

El estado del botón B (bState).

Esta información se envía como una cadena de texto, con cada valor separado por comas y al final un salto de línea (\n).

## ¿Qué significa esta parte del código?
python
Copiar
Editar
"{},{},{},{}\n".format(xValue, yValue, aState, bState)
Yo entendí que esta línea crea una cadena de texto donde se insertan los valores en el siguiente orden:

El valor del acelerómetro en X,

El valor del acelerómetro en Y,

El estado del botón A (True/False),

El estado del botón B (True/False).

El formato "{}" es un marcador de posición, y .format(...) reemplaza cada uno con los valores reales. Las comas separan los datos para que luego puedan ser fácilmente interpretados (por ejemplo, desde p5.js), y el \n (salto de línea) indica el final de la línea de datos.

##  ¿Cómo se ven los datos en la aplicación SerialTerminal?
Cuando observo los datos en la aplicación SerialTerminal, veo líneas como esta:

graphql
Copiar
Editar
120,-342,False,True
Esto confirma que los datos están separados por comas y terminan con un salto de línea. Así, cada línea representa un conjunto completo de datos enviados cada 100 ms.

 ## ¿Qué pasaría si no se separan con comas o no se usa salto de línea?
Yo creo que si no se usaran comas, los datos se mezclarían, por ejemplo:

Copiar
Editar
120-342FalseTrue
Eso sería muy difícil de interpretar desde otro software, como p5.js. Tampoco sabría dónde empieza y termina cada valor.

Si no se usara el salto de línea, los datos se concatenarían en una sola línea interminable:

graphql
Copiar
Editar
120,-342,False,True130,-320,False,False...
Eso dificultaría mucho la lectura, ya que no habría una marca clara de separación entre una lectura y la siguiente.

## ¿Para qué sirve sleep(100)?
Entendí que la función sleep(100) pausa el programa 100 milisegundos entre cada lectura y escritura. Esto significa que el micro:bit envía datos 10 veces por segundo (10 Hz).

Si no se usara esta pausa, el micro:bit enviaría datos lo más rápido posible, lo que podría saturar el canal UART y generar lecturas demasiado densas o errores en el software receptor (como p5.js).

## Observaciones de los valores xValue y yValue
Cuando inclino el micro:bit:

Hacia la derecha ➡️ xValue aumenta (valores positivos)

Hacia la izquierda ⬅️ xValue disminuye (valores negativos)

Hacia adelante ⬆️ yValue aumenta

Hacia atrás ⬇️ yValue disminuye

Los valores suelen estar entre -1024 y +1024, dependiendo de la inclinación.

## ¿Qué valores toman aState y bState?
Cuando presiono el botón A, aState cambia a True, y cuando lo suelto vuelve a False. Lo mismo ocurre con bState y el botón B.

## Diferencias entre is_pressed() y was_pressed()
is_pressed() devuelve True solo mientras el botón está siendo presionado.

was_pressed() devuelve True solo una vez, es decir, detecta el evento de haber sido presionado. Después de eso, vuelve a False, hasta que el botón se vuelva a presionar.

Esto significa que was_pressed() es útil para detectar eventos únicos, como hacer clic, mientras que is_pressed() es útil para detectar si el botón está mantenido presionado.

## Análisis de los datos enviados por UART
Si los datos fueran:

yaml
Copiar
Editar
xValue: 969, yValue: 652, aState: True, bState: False
Entonces la línea enviada sería:

graphql
Copiar
Editar
969,652,True,False\n
Eso se convierte en la siguiente secuencia de bytes ASCII:

57 (9)

54 (6)

57 (9)

2C (coma)

36 (6)

35 (5)

32 (2)

2C (coma)

54 72 75 65 (T r u e)

2C (coma)

46 61 6C 73 65 (F a l s e)

0A (salto de línea)

En hexadecimal, esto se vería como:

mathematica
Copiar
Editar
39 36 39 2C 36 35 32 2C 54 72 75 65 2C 46 61 6C 73 65 0A
Esto confirma que los datos enviados son puramente texto, y que el formato con comas y salto de línea es fundamental para estructurar correctamente la información.
