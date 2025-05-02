 ## 🧠 Actividad 04
Diseño técnico - arquitectura, flujos y protocolos
## Diagrama de Arquitectura

<sub>Puedes reemplazar esta imagen con una hecha en herramientas como Excalidraw, Lucidchart o Figma.</sub>

## Tecnologías de Comunicación
Origen	Destino	Tecnología	Protocolo/Formato
Micro:bit	Cliente p5.js	Web Serial API	ASCII (valores separados por coma, ej. "ax,ay,az,btnA,btnB")
Móvil	Servidor Node.js	Socket.IO (web)	JSON (ej: { "tap": true })
Servidor Node.js	Cliente p5.js	Socket.IO (web)	JSON (idéntico al que recibe)
Teclado/Mouse	Cliente p5.js	Eventos locales	JS events (keyPressed, mousePressed)

##Protocolos de Datos
Micro:bit → p5.js (Serial - ASCII):
Ejemplo de mensaje:

```swift

"0.25,0.12,0.98,1,0\n"
Formato:

ax,ay,az = acelerómetro

btnA, btnB = estado de botones (0 o 1)

Móvil → Servidor → p5.js (Socket.IO - JSON):
```
Ejemplo:

```json

{ "tap": true }
```
tap: detecta toque que puede activar un "boost" o "obstáculo"

Este mensaje se envía sin modificación desde el servidor

🧩 Esquema Lógico del Servidor Puente (Node.js)
```javascript
// Pseudocódigo simplificado de server.js

const io = require('socket.io')(server);

io.on('connection', (socket) => {
  socket.on('mobileInput', (data) => {
    // Solo reenvía sin modificar
    io.emit('mobileInput', data);
  });
});
```
✅ No guarda estado

✅ No modifica datos

✅ Solo retransmite

🧠 Esquema Lógico del Cliente p5.js (sketch.js)
```javascript


// Pseudocódigo resumido del cliente

// Variables de estado
let acelX, acelY, acelZ;
let btnA, btnB;
let tap = false;
let velocidad = 0;
let avance = 0;
let semana = 1;

// Setup
function setup() {
  createCanvas(800, 600);
  conectarSerialMicrobit(); // Web Serial
  conectarSocket();         // Socket.IO
}

// Procesamiento Serial
function recibirDatosSerial(data) {
  let valores = data.split(',');
  acelX = float(valores[0]);
  acelY = float(valores[1]);
  acelZ = float(valores[2]);
  btnA = int(valores[3]);
  btnB = int(valores[4]);
}

// Procesamiento de Socket
socket.on('mobileInput', (data) => {
  tap = data.tap;
});

// Eventos Locales
function keyPressed() {
  if (key == ' ') {
    velocidad += 1 + semana * 0.5; // aumenta dificultad por semana
  }
}

// Loop principal
function draw() {
  background(220);

  // Reglas del juego (Process)
  if (btnA) velocidad += 0.2;
  if (tap) velocidad *= 0.9; // penalización
  avance += velocidad;
  velocidad *= 0.95; // fricción

  // Dibujar output
  fill(0);
  textSize(32);
  text("Distancia: " + nf(avance, 1, 2), 50, 50);

  // Mostrar avatar
  ellipse(100 + avance, height/2, 50, 50);
}
```
✅ Conclusiones
El servidor Node.js solo retransmite datos móviles (tap).

La lógica y visualización se concentra totalmente en p5.js.

La entrada del micro:bit, móvil y teclado se combinan para un solo proceso coherente y con retroalimentación visual.

El output visual en p5.js refleja directamente el estado procesado (avance del personaje en función del input combinado).
