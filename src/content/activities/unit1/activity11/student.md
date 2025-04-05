### Programa en j5 que muestre un círculo en la pantalla. Utiliza los botones A y B para controlar la posición en x del círculo en el canvas de p5.js.
``` Js
let xPos;

function setup() {
  createCanvas(400, 400);
  xPos = width / 2; // Posición inicial del círculo en el centro
}

function draw() {
  background(220);
  
  // Dibujar el círculo
  fill(0, 102, 255);
  ellipse(xPos, height / 2, 50, 50);
}

// Conectar el micro:bit y recibir datos
function keyPressed() {
  if (key === 'A') {
    xPos -= 10; // Mover a la izquierda
  } else if (key === 'B') {
    xPos += 10; // Mover a la derecha
  }
}
```

codigo para microbit
``` js
from microbit import *

while True:
    if button_a.is_pressed():
        uart.write("A\n")  # Enviar 'A' si se presiona el botón A
        sleep(200)  # Pequeña pausa para evitar múltiples pulsaciones rápidas
    
    if button_b.is_pressed():
        uart.write("B\n")  # Enviar 'B' si se presiona el botón B
        sleep(200)
```

### El micro:bit detecta los botones

Cuando presionas el botón A, el micro:bit envía una señal con la letra "A".
Cuando presionas el botón B, el micro:bit envía una señal con la letra "B".
El micro:bit envía estos datos a la computadora

La computadora recibe estas letras a través del puerto serie (que es como un canal de comunicación).
p5.js lee los datos y mueve el círculo

p5.js está constantemente revisando si recibe una "A" o una "B".
Si recibe una "A", reduce la posición en X del círculo (moviéndolo a la izquierda).
Si recibe una "B", aumenta la posición en X del círculo (moviéndolo a la derecha).
