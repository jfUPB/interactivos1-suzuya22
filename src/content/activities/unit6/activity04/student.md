## Actividad 04: Explorando los clientes (p5.js + Socket.IO)
Para completar esta actividad, vamos a trabajar con las interacciones entre dos clientes en tiempo real utilizando Socket.IO. Aquí están los pasos y las explicaciones para cada una de las partes de la actividad:

1. Variables globales y conexión inicial
En el archivo page2.js, definimos las siguientes variables:

currentPageData: Almacena la información sobre la posición y tamaño de la ventana de la página.

remotePageData: Inicializa la posición de la ventana remota (page1) con valores predeterminados.

point2: Coordenadas para el centro de la ventana local.

socket: Establece la conexión con el servidor utilizando Socket.IO (io('http://localhost:3000')).

El cliente inicia la conexión al servidor y obtiene la información de la ventana.

```javascript

let currentPageData = { /* datos iniciales de la ventana */ };
let remotePageData = { x: 0, y: 0, width: 100, height: 100 };
let point2 = [currentPageData.width / 2, currentPageData.height / 2];

let socket = io('http://localhost:3000');
```
2. Manejando la conexión establecida
Aquí gestionamos el evento connect, que se dispara cuando la conexión al servidor se establece. Se emite un mensaje con los datos de la ventana al servidor para que la ventana remota tenga conocimiento del estado inicial de la ventana local.

```javascript

socket.on('connect', () => {
    console.log('Connected to server - My ID:', socket.id);
    socket.emit('win2update', currentPageData, socket.id);
});
```
3. Recibiendo datos del servidor
Cuando el servidor envía datos sobre la otra ventana (page1), el cliente page2 recibe esos datos mediante el evento getdata.

```javascript

socket.on('getdata', (dataWindow) => {
    remotePageData = dataWindow;
    console.log('Page 2 received data about the other window:', remotePageData);
});
```
4. Detectando cambios y enviando actualizaciones
Para detectar si la ventana local ha cambiado (movimiento o redimensionamiento), comparamos el estado actual con el anterior. Si detectamos un cambio, emitimos una actualización al servidor.

```javascript

let previousPageData = { /* datos iniciales */ };

function checkWindowPosition() {
    currentPageData = { /* obtener datos actuales de la ventana */ };

    if (currentPageData.x !== previousPageData.x || currentPageData.y !== previousPageData.y ||
        currentPageData.width !== previousPageData.width || currentPageData.height !== previousPageData.height) {
        
        console.log("Page 2 detected change, sending update:", currentPageData);
        point2 = [currentPageData.width / 2, currentPageData.height / 2];
        socket.emit('win2update', currentPageData, socket.id);
        previousPageData = currentPageData;
    }
}
```
5. La visualización (draw)
La función draw se ejecuta continuamente para actualizar la interfaz gráfica. Dibuja un círculo en el centro de la ventana local y otro en el centro relativo de la ventana remota.

```javascript

function draw() {
    background(220);
    drawCircle(point2[0], point2[1]); // Círculo en el centro de esta ventana
    checkWindowPosition();

    let vector1 = createVector(currentPageData.x, currentPageData.y);
    let vector2 = createVector(remotePageData.x, remotePageData.y);
    let resultingVector = createVector(vector2.x - vector1.x, vector2.y - vector1.y);

    stroke(50);
    strokeWeight(20);
    drawCircle(resultingVector.x + remotePageData.width / 2, resultingVector.y + remotePageData.height / 2);
    line(point2[0], point2[1], resultingVector.x + remotePageData.width / 2, resultingVector.y + remotePageData.height / 2);
}
```
