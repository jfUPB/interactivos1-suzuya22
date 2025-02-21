### crea un programa en p5.js que permita seleccionar 4 imágenes diferentes en el micro:bit.

``` Js
let images = [];
let currentImage = 0;

function preload() {
  images[0] = loadImage("img1.jpg");  // Reemplaza con tus imágenes
  images[1] = loadImage("img2.jpg");
  images[2] = loadImage("img3.jpg");
  images[3] = loadImage("img4.jpg");
}

function setup() {
  createCanvas(400, 400);
  noLoop();  // Solo dibuja cuando cambia la imagen

  // Conectar con el micro:bit usando Web Serial API
  navigator.serial.requestPort().then(port => {
    port.open({ baudRate: 115200 }).then(() => {
      let reader = port.readable.getReader();
      readLoop(reader);
    });
  });
}

function draw() {
  background(220);
  image(images[currentImage], 50, 50, 300, 300);
}

// Función para leer los datos del micro:bit
async function readLoop(reader) {
  while (true) {
    const { value, done } = await reader.read();
    if (done) break;
    let data = new TextDecoder().decode(value).trim();

    if (data === "1") currentImage = 0;
    else if (data === "2") currentImage = 1;
    else if (data === "3") currentImage = 2;
    else currentImage = 3;  // Imagen por defecto

    redraw();
  }
}
```

### codigo microbit
``` js
from microbit import *

uart.init(baudrate=115200)  # Habilita la comunicación serial

while True:
    if button_a.is_pressed() and button_b.is_pressed():
        uart.write("3\n")  # Enviar "3" si se presionan A y B juntos
    elif button_a.is_pressed():
        uart.write("1\n")  # Enviar "1" si se presiona A
    elif button_b.is_pressed():
        uart.write("2\n")  # Enviar "2" si se presiona B
    else:
        uart.write("0\n")  # Enviar "0" si no se presiona nada

    sleep(200)  # Pausa para evitar envíos rápidos
```

### Cómo solucioné el problema de seleccionar imágenes con el micro:bit en p5.js
Solucioné el problema haciendo que el micro:bit enviara un número por el puerto serie según el botón presionado (A, B o ambos). En p5.js, 
usé la Web Serial API para recibir estos datos y actualizar la imagen mostrada en pantalla. Dependiendo del número recibido, cambié la imagen usando image() y redraw(), permitiendo que los botones del micro:bit actúen como un control remoto para seleccionar imágenes en la web.
