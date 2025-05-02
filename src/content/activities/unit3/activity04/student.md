# Actividad seriales

## En p5.js:

```js
let port;
let writer;

async function connectSerial() {
  try {
    port = await navigator.serial.requestPort();
    await port.open({ baudRate: 115200 });

    writer = port.writable.getWriter();
    console.log("✅ Conectado al puerto serial");
  } catch (error) {
    console.error("❌ Error al conectar al puerto serial:", error);
  }
}

function setup() {
  createCanvas(400, 200);
  let button = createButton("Conectar Serial");
  button.position(10, 10);
  button.mousePressed(connectSerial);
}

function draw() {
  background(220);
  textAlign(CENTER, CENTER);
  textSize(20);
  text("Presiona A, B, S o T para enviar eventos", width / 2, height / 2);
}

async function keyPressed(event) {
  let evento = "";

  if (event.key === "a") {
    evento = "A";
  } else if (event.key === "b") {
    evento = "B";
  } else if (event.key === "s") {
    evento = "S";
  } else if (event.key === "t") {
    evento = "T";
  }

  console.log("🔹 Tecla presionada:", event.key); // Verificar que detecta la tecla

  if (evento !== "") {
    console.log("📤 Enviando evento:", evento);

    if (!writer) {
      console.log("⚠️ No hay conexión serial");
      return;
    }

    try {
      const data = new TextEncoder().encode(evento + "\n");
      await writer.write(data);
      console.log("✅ Enviado:", evento);
    } catch (error) {
      console.error("❌ Error al enviar datos:", error);
    }
  }
}
```

## En micro python:

```py
let port;
let writer;

async function connectSerial() {
  try {
    port = await navigator.serial.requestPort();
    await port.open({ baudRate: 115200 });

    writer = port.writable.getWriter();
    console.log("✅ Conectado al puerto serial");
  } catch (error) {
    console.error("❌ Error al conectar al puerto serial:", error);
  }
}

function setup() {
  createCanvas(400, 200);
  let button = createButton("Conectar Serial");
  button.position(10, 10);
  button.mousePressed(connectSerial);
}

function draw() {
  background(220);
  textAlign(CENTER, CENTER);
  textSize(20);
  text("Presiona A, B, S o T para enviar eventos", width / 2, height / 2);
}

async function keyPressed(event) {
  let evento = "";

  if (event.key === "a") {
    evento = "A";
  } else if (event.key === "b") {
    evento = "B";
  } else if (event.key === "s") {
    evento = "S";
  } else if (event.key === "t") {
    evento = "T";
  }

  console.log("🔹 Tecla presionada:", event.key); // Verificar que detecta la tecla

  if (evento !== "") {
    console.log("📤 Enviando evento:", evento);

    if (!writer) {
      console.log("⚠️ No hay conexión serial");
      return;
    }

    try {
      const data = new TextEncoder().encode(evento + "\n");
      await writer.write(data);
      console.log("✅ Enviado:", evento);
    } catch (error) {
      console.error("❌ Error al enviar datos:", error);
    }
  }
}
```
