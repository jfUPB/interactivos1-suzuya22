## Actividad 05
Inputs y el puente Node.js
## Estructura del Proyecto
```plaintext

/mi-proyecto
├── public
│   ├── desktop
│   │   └── sketch.js       # Cliente p5.js
│   └── mobile
│       └── sketch.js       # Cliente móvil (touch input)
├── server.js               # Servidor puente
└── index.html              # Archivo HTML para desktop
```
📡 Fragmentos Clave
🧱 server.js (Servidor Node.js Puente)
```js

const express = require('express');
const app = express();
const http = require('http').createServer(app);
const io = require('socket.io')(http);

app.use(express.static('public'));

io.on('connection', (socket) => {
  console.log('🟢 Cliente conectado');

  socket.on('mobileInput', (data) => {
    console.log('📲 Input móvil recibido:', data);
    io.emit('mobileInputForwarded', data);
  });
});

http.listen(3000, () => {
  console.log('Servidor corriendo en http://localhost:3000');
});
```
📱 public/mobile/sketch.js (Cliente móvil)
```js

let socket;

function setup() {
  createCanvas(windowWidth, windowHeight);
  socket = io();

  // Simulación de toque
  createButton('Enviar toque').position(100, 100).mousePressed(() => {
    socket.emit('mobileInput', { tap: true });
  });
}
```
🧠 public/desktop/sketch.js (Cliente p5.js)
```js

let socket;
let serial;  // Objeto Web Serial
let serialData = '';

function setup() {
  createCanvas(600, 400);
  connectSerial();
  connectSocket();
}

function connectSocket() {
  socket = io();

  socket.on('mobileInputForwarded', (data) => {
    console.log('📲 Datos desde el móvil:', data);
  });
}

function connectSerial() {
  serial = new p5.SerialPort();

  serial.on('list', console.log);
  serial.on('connected', () => console.log('🔌 Conectado a Web Serial'));
  serial.on('data', () => {
    serialData = serial.readLine().trim();
    console.log('🎛 Datos desde micro:bit:', serialData);
  });

  serial.list();  // Mostrar lista de puertos
  serial.openPrompt(); // Solicitar conexión al usuario
}
```
🔧 Código micro:bit (MakeCode / JavaScript)
```javascript

basic.forever(function () {
    let ax = input.acceleration(Dimension.X)
    let ay = input.acceleration(Dimension.Y)
    let az = input.acceleration(Dimension.Z)
    let btnA = input.buttonIsPressed(Button.A) ? 1 : 0
    let btnB = input.buttonIsPressed(Button.B) ? 1 : 0
    serial.writeLine("" + ax + "," + ay + "," + az + "," + btnA + "," + btnB)
    basic.pause(100)
})
```
🧪 Evidencia: Logs de la Consola (Cliente p5.js)
```yaml

🎛 Datos desde micro:bit: -13,1023,-7,0,0
📲 Datos desde el móvil: { tap: true }
```
## Desafíos Encontrados
Problema	Solución Aplicada
Web Serial no disponible en Firefox	Cambié a Google Chrome para usar Web Serial API
El servidor no reenviaba datos	Verifiqué que los nombres de eventos coincidan (mobileInput, mobileInputForwarded)
El micro:bit enviaba líneas vacías	Añadí .trim() en serial.readLine() para ignorar líneas vacías
Error CORS desde el móvil	Usé Dev Tunnels (Visual Studio Code) o ngrok para exponer el servidor a internet si se usaba móvil real

## Conclusión
El micro:bit envía datos por Serial y se registran correctamente en sketch.js.

El cliente móvil emite eventos con Socket.IO que son reenviados por el servidor.

El cliente p5.js recibe ambos tipos de datos y los muestra en consola, lo que confirma que los inputs están conectados correctamente.

La estructura de comunicación básica ya está funcionando y lista para integrar el "Process".

