## ACTIVIDAD BOMBA 2.0

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
ultima_accion = None
secuencia_correcta = ["A", "B", "A", "shake"]
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
    tiempo_restante = max(0, tiempo_restante - tiempo_transcurrido)
    
    if tiempo_restante <= 0:
        estado_actual = EXPLOSION_STATE
        display.show(Image.SKULL)
    else:
        # Mostrar barra de progreso
        leds = min(5, math.ceil(tiempo_restante/4))
        for i in range(5):
            display.set_pixel(i, 2, 9 if i < leds else 0)

def verificar_secuencia():
    global estado_actual, tiempo_restante, secuencia_usuario
    
    if len(secuencia_usuario) >= len(secuencia_correcta):
        if secuencia_usuario[-4:] == secuencia_correcta:
            # ¡Secuencia correcta!
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

# Bucle principal
while True:
    # 1. Modo Configuración
    if estado_actual == CONFIGURATION_MODE:
        if button_a.was_pressed() and button_b.was_pressed():
            iniciar_cuenta_regresiva()
        elif button_a.was_pressed():
            if tiempo_restante < 60:
                tiempo_restante += 1
                mostrar_tiempo()
        elif button_b.was_pressed():
            if tiempo_restante > 10:
                tiempo_restante -= 1
                mostrar_tiempo()
    
    # 2. Modo Cuenta Regresiva
    elif estado_actual == COUNTDOWN_MODE:
        actualizar_cuenta_regresiva()
        
        # Detectar acciones para la secuencia
        if button_a.was_pressed():
            secuencia_usuario.append("A")
        elif button_b.was_pressed():
            secuencia_usuario.append("B")
        elif accelerometer.was_gesture("shake"):
            secuencia_usuario.append("shake")
        
        # Verificar cada nueva acción
        if len(secuencia_usuario) >= len(secuencia_correcta):
            verificar_secuencia()
    
    # 3. Modo Explosión
    elif estado_actual == EXPLOSION_STATE:
        if button_a.was_pressed() and button_b.was_pressed():
            estado_actual = CONFIGURATION_MODE
            tiempo_restante = 20
            display.show(Image.ARROW_S)
            sleep(1000)
            mostrar_tiempo()
    
    sleep(100)
```

### etección en tiempo real:
Para que el micro:bit pueda reconocer cuando presionamos los botones o lo agitamos, usamos funciones específicas que vienen integradas en el sistema. Por ejemplo, button_a.was_pressed() devuelve True solo si el botón A fue presionado recientemente, lo mismo pasa con button_b.was_pressed() para el botón B, y accelerometer.was_gesture("shake") detecta si movimos bruscamente el dispositivo. Estas funciones son ideales porque no se activan por error; solo responden cuando realmente ocurre la acción que estamos buscando. Así evitamos falsos positivos y aseguramos que el programa reaccione únicamente cuando el usuario interactúa de manera intencional con la bomba.

### Almacenamiento de la secuencia:
Cada vez que el usuario presiona un botón o agita el micro:bit, el programa guarda esa acción en una lista llamada secuencia_usuario. Por ejemplo, si primero presiona A, luego B, después A otra vez y finalmente lo agita, la lista quedará así: ["A", "B", "A", "shake"]. Esta lista se va construyendo paso a paso con cada interacción válida, lo que nos permite rastrear exactamente lo que hizo el usuario en orden. Si el usuario presiona otros botones que no son parte de la secuencia, no importa, porque solo nos interesa detectar el patrón correcto en algún momento dentro de sus acciones.

### Verificación inteligente:
En lugar de estar revisando constantemente si la secuencia es correcta, el programa solo hace la comparación cuando hay suficientes acciones almacenadas (en este caso, cuatro). Usamos un truco muy útil en programación: secuencia_usuario[-4:] nos permite analizar solo los últimos cuatro elementos de la lista, sin importar si hay más botones presionados antes o después. Esto significa que el usuario puede equivocarse, presionar botones de más o incluso empezar a intentar la secuencia varias veces, y el sistema seguirá funcionando bien. Solo cuando esos últimos cuatro movimientos coincidan exactamente con la secuencia esperada, la bomba se desactivará.

### Retroalimentación visual:
Para que el usuario sepa si lo hizo bien o mal, el micro:bit muestra una carita feliz (con Image.YES) si la secuencia fue correcta, dándole la confirmación de que la bomba se desactivó. Si la secuencia está mal, aparece una carita triste (con Image.NO) para indicarle que debe intentarlo de nuevo. Esta respuesta inmediata es clave, porque sin ella el usuario no sabría si sus acciones están siendo registradas o si cometió un error. Además, como la pantalla del micro:bit es pequeña, usar íconos claros como caritas hace que la información sea fácil de entender incluso en medio de la cuenta regresiva.
