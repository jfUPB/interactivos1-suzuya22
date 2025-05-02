## 🧪 Actividad 04 - Aplica lo aprendido
🎯 Enunciado
Modificar la misma aplicación de la fase de aplicación de la unidad anterior para que soporte el protocolo de datos binarios.

La aplicación del micro:bit permanece igual a la de la Actividad 02:

```python

from microbit import *
import struct

uart.init(115200)
display.set_pixel(0, 0, 9)

while True:
    xValue = accelerometer.get_x()
    yValue = accelerometer.get_y()
    aState = button_a.is_pressed()
    bState = button_b.is_pressed()
```
    # Empaqueta los datos: 2 enteros (16 bits) y 2 bytes
    data = struct.pack('>2h2B', xValue, yValue, int(aState), int(bState))
    
    # Calcula checksum: suma de todos los bytes módulo 256
    checksum = sum(data) % 256
    
    # Paquete final con encabezado, datos y checksum
    packet = b'\xAA' + data + bytes([checksum])
    uart.write(packet)
    sleep(100)  # 10 veces por segundo
##  🧩 Enlace a la aplicación original (versión ASCII)
🔗 Enlace al proyecto anterior en p5.js:
https://editor.p5js.org/usuario_original
(Reemplazar con tu enlace real si aún no lo has subido)

##  🛠 Código de la aplicación modificada en p5.js (versión binaria)
```javascript

let serial;
let x, y, a, b;

function setup() {
  createCanvas(400, 400);
  serial = new p5.SerialPort();
  serial.open('/dev/ttyUSB0'); // Cambia esto según tu puerto
  serial.on('data', onSerialData);
}

function draw() {
  background(220);
  textSize(20);
  text(`x: ${x}`, 20, 40);
  text(`y: ${y}`, 20, 70);
  text(`A: ${a}`, 20, 100);
  text(`B: ${b}`, 20, 130);
}

function onSerialData() {
  let inBytes = serial.readBytes();

  // Verificar si hay al menos 8 bytes (1 encabezado + 6 datos + 1 checksum)
  for (let i = 0; i < inBytes.length - 7; i++) {
    if (inBytes[i] === 0xAA) { // Encabezado correcto
      let data = inBytes.slice(i + 1, i + 7);
      let checksum = inBytes[i + 7];
      let sum = data.reduce((a, b) => a + b, 0) % 256;

      if (sum === checksum) {
        let xRaw = (data[0] << 8) | data[1];
        let yRaw = (data[2] << 8) | data[3];
        // Convertir a signed 16-bit
        if (xRaw >= 0x8000) xRaw -= 0x10000;
        if (yRaw >= 0x8000) yRaw -= 0x10000;
        x = xRaw;
        y = yRaw;
        a = data[4];
        b = data[5];
        break;
      }
    }
  }
}
```
🔗 Enlace a la aplicación modificada en p5.js
🆕 Proyecto p5.js con soporte para datos binarios:
https://editor.p5js.org/suzuya22/sketches/BSbwVCrEx
