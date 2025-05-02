SEMAFORO ACTIVIDAD 

CODIGO 
``` js
from microbit import *

class Semaforo:
    def __init__(self, x, y, t_rojo, t_amarillo, t_verde):
        self.x = x  # Posición x en la matriz de leds
        self.y = y  # Posición y en la matriz de leds
        self.t_rojo = t_rojo
        self.t_amarillo = t_amarillo
        self.t_verde = t_verde
        self.estado = "rojo"
        self.tiempo_restante = t_rojo
        self.contador = 0
    
    def update(self):
        self.contador += 100  # Asumimos que se llama cada 100ms
        
        if self.contador >= 1000:  # Cada segundo
            self.contador = 0
            self.tiempo_restante -= 1
            
            if self.tiempo_restante <= 0:
                self.cambiar_estado()
    
    def cambiar_estado(self):
        if self.estado == "rojo":
            self.estado = "amarillo"
            self.tiempo_restante = self.t_amarillo
        elif self.estado == "amarillo":
            self.estado = "verde"
            self.tiempo_restante = self.t_verde
        else:  # verde
            self.estado = "rojo"
            self.tiempo_restante = self.t_rojo
    
    def dibujar(self):
        # Representamos los colores con diferentes intensidades
        if self.estado == "rojo":
            display.set_pixel(self.x, self.y, 9)  # Máxima intensidad
        elif self.estado == "amarillo":
            display.set_pixel(self.x, self.y, 5)  # Intensidad media
        else:  # verde
            display.set_pixel(self.x, self.y, 1)  # Intensidad baja

# Crear los tres semáforos con sus tiempos específicos
semaforo1 = Semaforo(1, 1, 5, 2, 3)
semaforo2 = Semaforo(1, 3, 3, 1, 2)
semaforo3 = Semaforo(3, 3, 4, 3, 2)

while True:
    # Actualizar todos los semáforos
    semaforo1.update()
    semaforo2.update()
    semaforo3.update()
    
    # Limpiar pantalla y dibujar semáforos
    display.clear()
    semaforo1.dibujar()
    semaforo2.dibujar()
    semaforo3.dibujar()
    
    # Esperar 100ms antes de la próxima actualización
    sleep(100)
```


## Reflexión sobre el uso de clases y máquinas de estado

Al principio, cuando vi el problema de los tres semáforos, me pareció complicado manejar tantos tiempos y estados diferentes a la vez. Pero al usar una clase para cada semáforo, todo se hizo más ordenado. Cada uno lleva su propio control, como si fuera una persona independiente siguiendo su rutina, sin liarse con los demás. Esto me hizo entender por qué en programación se usan objetos: porque imitan cómo funcionan las cosas en la vida real. Un semáforo no necesita saber qué hace el otro; solo sigue sus reglas. Además, si quiero cambiar algo, como el tiempo del amarillo, solo modifico ese semáforo en concreto y no me preocupo por romper el resto.

Lo de las máquinas de estado al principio sonaba a algo muy técnico, pero en realidad es como un ciclo de vida. El semáforo no está "inventando" qué hacer; solo pasa por sus fases (rojo, amarillo, verde) una y otra vez, como cuando nosotros dormimos, comemos y estudiamos en un orden lógico. Lo bueno es que, aunque parezca simple, esta estructura ayuda a que el programa no se vuelva un lío de condiciones y contadores mezclados. Al final, aprendí que programar no es solo escribir código, sino pensar en cómo organizar las ideas para que todo funcione sin volvernos locos.
