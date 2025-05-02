## Codigo bomba en p5.js

// Estados de la bomba
const CONFIGURATION_MODE = 0;
const COUNTDOWN_MODE = 1;
const EXPLOSION_STATE = 2;

// Variables globales
let estadoActual = CONFIGURATION_MODE;
let tiempoRestante = 20;
let startTime = 0;
let secuenciaCorrecta = ['A', 'B', 'A', 'S'];
let secuenciaUsuario = [];
let ultimoEvento = null;
let hayNuevoEvento = false;

// Tamaños y posiciones
let btnA, btnB, btnShake, btnTouch;
let barWidth = 300;
let barHeight = 30;

function setup() {
  createCanvas(400, 400);
  textAlign(CENTER, CENTER);
  textSize(24);
  
  // Crear botones simulados
  btnA = createButton('Botón A');
  btnA.position(50, 350);
  btnA.mousePressed(() => registrarEvento('A'));
  
  btnB = createButton('Botón B');
  btnB.position(150, 350);
  btnB.mousePressed(() => registrarEvento('B'));
  
  btnShake = createButton('Shake');
  btnShake.position(250, 350);
  btnShake.mousePressed(() => registrarEvento('S'));
  
  btnTouch = createButton('Touch');
  btnTouch.position(350, 350);
  btnTouch.mousePressed(() => registrarEvento('T'));
}

function draw() {
  background(240);
  
  // Mostrar estado actual
  fill(0);
  text(`Estado: ${obtenerNombreEstado(estadoActual)}`, width/2, 30);
  text(`Tiempo: ${tiempoRestante} seg`, width/2, 60);
  
  // Dibujar según el estado
  if (estadoActual === CONFIGURATION_MODE) {
    dibujarModoConfiguracion();
  } else if (estadoActual === COUNTDOWN_MODE) {
    dibujarModoCuentaRegresiva();
  } else {
    dibujarModoExplosion();
  }
  
  // Procesar eventos
  procesarEventos();
}

function registrarEvento(evento) {
  ultimoEvento = evento;
  hayNuevoEvento = true;
}

function procesarEventos() {
  if (!hayNuevoEvento) return;
  
  if (estadoActual === CONFIGURATION_MODE) {
    if (ultimoEvento === 'A') {
      if (tiempoRestante < 60) tiempoRestante++;
    } else if (ultimoEvento === 'B') {
      if (tiempoRestante > 10) tiempoRestante--;
    } else if (ultimoEvento === 'A' && keyIsDown(66)) { // A+B
      iniciarCuentaRegresiva();
    }
  } 
  else if (estadoActual === COUNTDOWN_MODE) {
    secuenciaUsuario.push(ultimoEvento);
    verificarSecuencia();
  }
  else if (estadoActual === EXPLOSION_STATE && ultimoEvento === 'T') {
    reiniciarBomba();
  }
  
  hayNuevoEvento = false;
}

function dibujarModoConfiguracion() {
  fill(100);
  text("Modo Configuración", width/2, 100);
  text("A: +1s | B: -1s | A+B: Iniciar", width/2, 130);
}

function dibujarModoCuentaRegresiva() {
  // Barra de progreso
  let progress = map(tiempoRestante, 20, 0, 0, barWidth);
  fill(255, 0, 0);
  rect(width/2 - barWidth/2, 150, barWidth, barHeight);
  fill(0, 255, 0);
  rect(width/2 - barWidth/2, 150, barWidth - progress, barHeight);
  
  // Secuencia ingresada
  fill(0);
  text(`Secuencia: ${secuenciaUsuario.join(' ')}`, width/2, 200);
}

function dibujarModoExplosion() {
  fill(255, 0, 0);
  textSize(32);
  text("¡BOOM!", width/2, 150);
  textSize(24);
  text("Presiona Touch para reiniciar", width/2, 200);
}

function iniciarCuentaRegresiva() {
  estadoActual = COUNTDOWN_MODE;
  startTime = millis();
}

function actualizarCuentaRegresiva() {
  let tiempoTranscurrido = floor((millis() - startTime) / 1000);
  tiempoRestante = max(0, 20 - tiempoTranscurrido);
  
  if (tiempoRestante <= 0) {
    estadoActual = EXPLOSION_STATE;
  }
}

function verificarSecuencia() {
  if (secuenciaUsuario.length >= 4) {
    let ultimos4 = secuenciaUsuario.slice(-4);
    if (JSON.stringify(ultimos4) === JSON.stringify(secuenciaCorrecta)) {
      estadoActual = CONFIGURATION_MODE;
      tiempoRestante = 20;
      secuenciaUsuario = [];
    } else {
      secuenciaUsuario = [];
    }
  }
}

function reiniciarBomba() {
  estadoActual = CONFIGURATION_MODE;
  tiempoRestante = 20;
  secuenciaUsuario = [];
}

function obtenerNombreEstado(estado) {
  switch(estado) {
    case CONFIGURATION_MODE: return "Configuración";
    case COUNTDOWN_MODE: return "Cuenta Regresiva";
    case EXPLOSION_STATE: return "Explosión";
    default: return "Desconocido";
  }
}

function keyPressed() {
  if (key === 'a' || key === 'A') registrarEvento('A');
  if (key === 'b' || key === 'B') registrarEvento('B');
  if (key === 's' || key === 'S') registrarEvento('S');
  if (key === 't' || key === 'T') registrarEvento('T');
}
