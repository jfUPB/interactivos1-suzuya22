# Análisis del Cliente Móvil y de Escritorio

## Propósito de `mobile/sketch.js` y `desktop/sketch.js`

El propósito principal de **`mobile/sketch.js`** es capturar los toques del usuario en un dispositivo móvil (como un smartphone o tablet) y enviar esos datos de posición (coordenadas `x` e `y`) al servidor a través de Socket.IO. Esto se logra utilizando la función `touchMoved()` de p5.js, que detecta el movimiento del dedo sobre el canvas.

Por otro lado, **`desktop/sketch.js`** es responsable de recibir esos datos desde el servidor, interpretar las coordenadas del toque (en formato JSON) y actualizar la posición de un círculo en el canvas del cliente de escritorio. El círculo se mueve en función de las coordenadas recibidas, lo que simula la interacción entre el dispositivo móvil y el escritorio.

## Cliente Móvil (`mobile/sketch.js`)

### Explicación del Código (Móvil)

```javascript
let socket;
let lastTouchX = null;
let lastTouchY = null;
const threshold = 5; // Umbral para evitar enviar demasiados mensajes

function setup() {
    socket = io();
    socket.on('connect', () => console.log('Connected to server'));
}

function touchMoved() {
    if (socket && socket.connected) {
        let dx = abs(mouseX - lastTouchX);
        let dy = abs(mouseY - lastTouchY);

        if (dx > threshold || dy > threshold || lastTouchX === null) {
            let touchData = {
                type: 'touch', // Tipo de mensaje
                x: mouseX,     // Coordenada X del toque
                y: mouseY      // Coordenada Y del toque
            };
            socket.emit('message', JSON.stringify(touchData));

            lastTouchX = mouseX;
            lastTouchY = mouseY;
        }
    }
    return false; // Evita comportamiento por defecto del navegador en móviles
}
