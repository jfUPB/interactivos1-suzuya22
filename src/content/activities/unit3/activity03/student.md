## Actividad Bomba 3.0
```js
from microbit import *
import math

# --- Estados de la bomba ---
CONFIGURATION_MODE = 0
COUNTDOWN_MODE = 1
EXPLOSION_STATE = 2

# --- Variables globales ---
estado_actual = CONFIGURATION_MODE
tiempo_restante = 20
start_time = 0
secuencia_correcta = ["A", "B", "A", "S"]  # A, B, A, Shake
secuencia_usuario = []

# --- Sistema de eventos ---
evento_actual = None
nuevo_evento = False

# --- Funciones de la bomba ---
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
    tiempo_restante = max(0, tiempo_restante - tiempo_transcurrido)
    
    if tiempo_restante <= 0:
        estado_actual = EXPLOSION_STATE
        display.show(Image.SKULL)
    else:
        # Barra de progreso en la fila central
        leds = min(5, math.ceil(tiempo_restante / 4))
        for i in range(5):
            display.set_pixel(i, 2, 9 if i < leds else 0)

def verificar_secuencia():
    global estado_actual, tiempo_restante, secuencia_usuario
    
    if len(secuencia_usuario) >= len(secuencia_correcta):
        if secuencia_usuario[-4:] == secuencia_correcta:
            # ¡Secuencia correcta! Desactivar bomba.
            estado_actual = CONFIGURATION_MODE
            tiempo_restante = 20
            secuencia_usuario = []
            display.show(Image.YES)
            sleep(1000)
            mostrar_tiempo()
        else:
            # Secuencia incorrecta
            display.show(Image.NO)
            sleep(500)
            display.clear()

# --- Tarea de la bomba (máquina de estados) ---
def tareaBomba():
    global nuevo_evento, secuencia_usuario
    
    # 1. Modo Configuración
    if estado_actual == CONFIGURATION_MODE:
        if nuevo_evento:
            if evento_actual == 'A':
                if tiempo_restante < 60:
                    tiempo_restante += 1
                    mostrar_tiempo()
            elif evento_actual == 'B':
                if tiempo_restante > 10:
                    tiempo_restante -= 1
                    mostrar_tiempo()
            nuevo_evento = False  # Consumir evento
    
    # 2. Modo Cuenta Regresiva
    elif estado_actual == COUNTDOWN_MODE:
        actualizar_cuenta_regresiva()
        
        if nuevo_evento:
            secuencia_usuario.append(evento_actual)
            verificar_secuencia()
            nuevo_evento = False  # Consumir evento
    
    # 3. Modo Explosión
    elif estado_actual == EXPLOSION_STATE:
        if nuevo_evento and evento_actual == 'T':  # Touch para reiniciar
            estado_actual = CONFIGURATION_MODE
            tiempo_restante = 20
            display.show(Image.ARROW_S)
            sleep(1000)
            mostrar_tiempo()
            nuevo_evento = False

# --- Tarea de eventos (sensores + serial) ---
def tareaEventos():
    global evento_actual, nuevo_evento
    
    # 1. Eventos desde botones físicos
    if button_a.was_pressed():
        evento_actual = 'A'
        nuevo_evento = True
    elif button_b.was_pressed():
        evento_actual = 'B'
        nuevo_evento = True
    elif accelerometer.was_gesture("shake"):
        evento_actual = 'S'
        nuevo_evento = True
    elif pin_logo.is_touched():
        evento_actual = 'T'
        nuevo_evento = True
    
    # 2. Eventos desde el puerto serial
    mensaje = uart.readline()
    if mensaje:
        comando = mensaje.decode('utf-8').strip()
        if comando in ['A', 'B', 'S', 'T']:
            evento_actual = comando
            nuevo_evento = True

# --- Configuración inicial ---
uart.init(baudrate=115200)  # Habilitar comunicación serial

# --- Bucle principal ---
while True:
    tareaBomba()  # Maneja la lógica de la bomba
    tareaEventos()  # Detecta eventos (sensores + serial)
    sleep(100)
```

### ¿Cómo funciona el sistema de eventos?
1. Detección de eventos (tareaEventos)
Botones A/B: Si se presionan, generan eventos 'A' o 'B'.

Shake (agitación): Genera evento 'S'.

Touch (pin logo): Genera evento 'T' (para reiniciar después de la explosión).

Puerto serial: Si se envía A, B, S o T desde la app serial, se procesa igual que un evento físico.
