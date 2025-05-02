## Controlador tambien desde el microbit

##  p5.js
```js
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

```
## codigo microbit
```js
from microbit import *
import math

# Estados
CONFIGURATION_MODE = 0
COUNTDOWN_MODE = 1
EXPLOSION_STATE = 2

# Variables globales
estado_actual = CONFIGURATION_MODE
tiempo_restante = 20
start_time = 0
secuencia_correcta = ['A', 'B', 'A', 'S']
secuencia_usuario = []

def mostrar_tiempo():
    display.scroll(str(tiempo_restante), delay=80)

def iniciar_cuenta_regresiva():
    global estado_actual, start_time
    estado_actual = COUNTDOWN_MODE
    start_time = running_time()
    display.show(Image.ARROW_N)

def actualizar_cuenta_regresiva():
    global estado_actual, tiempo_restante
    tiempo_transcurrido = (running_time() - start_time) // 1000
    tiempo_restante = max(0, 20 - tiempo_transcurrido)
    
    if tiempo_restante <= 0:
        estado_actual = EXPLOSION_STATE
        display.show(Image.SKULL)
    else:
        leds = min(5, math.ceil(tiempo_restante / 4))
        for i in range(5):
            display.set_pixel(i, 2, 9 if i < leds else 0)

def verificar_secuencia():
    global estado_actual, tiempo_restante, secuencia_usuario
    if len(secuencia_usuario) >= 4:
        if secuencia_usuario[-4:] == secuencia_correcta:
            estado_actual = CONFIGURATION_MODE
            tiempo_restante = 20
            secuencia_usuario = []
            display.show(Image.YES)
            sleep(1000)
            mostrar_tiempo()
        else:
            display.show(Image.NO)
            sleep(500)
            display.clear()

uart.init(baudrate=115200)

while True:
    # Leer eventos locales
    if button_a.was_pressed():
        uart.write('A\n')
        if estado_actual == CONFIGURATION_MODE:
            tiempo_restante = min(60, tiempo_restante + 1)
            mostrar_tiempo()
        elif estado_actual == COUNTDOWN_MODE:
            secuencia_usuario.append('A')
            verificar_secuencia()
            
    if button_b.was_pressed():
        uart.write('B\n')
        if estado_actual == CONFIGURATION_MODE:
            tiempo_restante = max(10, tiempo_restante - 1)
            mostrar_tiempo()
        elif estado_actual == COUNTDOWN_MODE:
            secuencia_usuario.append('B')
            verificar_secuencia()
    
    if accelerometer.was_gesture('shake'):
        uart.write('S\n')
        if estado_actual == COUNTDOWN_MODE:
            secuencia_usuario.append('S')
            verificar_secuencia()
    
    if pin_logo.is_touched():
        uart.write('T\n')
        if estado_actual == EXPLOSION_STATE:
            estado_actual = CONFIGURATION_MODE
            tiempo_restante = 20
            display.show(Image.ARROW_S)
            sleep(1000)
            mostrar_tiempo()
    
    # Leer eventos seriales
    mensaje = uart.readline()
    if mensaje:
        comando = mensaje.decode('utf-8').strip()
        if comando in ['A', 'B', 'S', 'T']:
            if estado_actual == CONFIGURATION_MODE:
                if comando == 'A': tiempo_restante = min(60, tiempo_restante + 1)
                if comando == 'B': tiempo_restante = max(10, tiempo_restante - 1)
                mostrar_tiempo()
            elif estado_actual == COUNTDOWN_MODE:
                secuencia_usuario.append(comando)
                verificar_secuencia()
            elif estado_actual == EXPLOSION_STATE and comando == 'T':
                estado_actual = CONFIGURATION_MODE
                tiempo_restante = 20
                display.show(Image.ARROW_S)
                sleep(1000)
                mostrar_tiempo()
    
    # Actualizar cuenta regresiva
    if estado_actual == COUNTDOWN_MODE:
        actualizar_cuenta_regresiva()
    
    sleep(100)
