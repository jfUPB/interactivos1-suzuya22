## Explicación: Concurrencia en el Programa

Este programa implementa una máquina de estados finitos para gestionar dos procesos concurrentes:

Secuencia de Imágenes: El programa muestra distintas expresiones faciales en el micro:bit según un ciclo de tiempo predefinido.

Interacción del Usuario: Permite que el usuario interrumpa la secuencia en cualquier momento presionando el botón A, lo que cambia inmediatamente la expresión mostrada.

### Manejo de concurrencia

La concurrencia se logra mediante una estructura de máquina de estados donde el programa evalúa constantemente dos eventos:

Si ha transcurrido el tiempo necesario para cambiar de expresión según el ciclo predefinido.

Si el usuario ha presionado el botón A para interrumpir la secuencia y cambiar la imagen.

Ambos eventos pueden ocurrir simultáneamente o en cualquier orden. El uso de utime.ticks_ms() permite gestionar el tiempo de manera precisa sin pausar la ejecución del programa, asegurando que el micro:bit pueda responder rápidamente a la entrada del usuario.

Vectores de Prueba

### Prueba 1: Cambio de estados sin interrupciones

Condiciones iniciales: El micro:bit está en el estado STATE_HAPPY.

Eventos generados: Se deja que el programa cambie de estado según los tiempos establecidos sin presionar el botón A.

### Resultados esperados:

Se muestra Image.HAPPY durante 1.5 segundos.

Se muestra Image.SMILE durante 1 segundo.

Se muestra Image.SAD durante 2 segundos.

Se repite el ciclo.

### Resultados obtenidos: El programa sigue la secuencia esperada sin errores.

✅ Prueba superada.

### Prueba 2: Interrupción en STATE_HAPPY

Condiciones iniciales: El micro:bit está en el estado STATE_HAPPY.

Eventos generados: Se presiona el botón A mientras Image.HAPPY está en pantalla.

Resultados esperados:

La imagen cambia inmediatamente a Image.SAD.

Image.SAD se muestra durante 2 segundos.

Luego, el ciclo sigue normalmente con Image.HAPPY.

Resultados obtenidos: El cambio a Image.SAD ocurre instantáneamente al presionar el botón A, y el ciclo continua correctamente.

✅ Prueba superada.

### Prueba 3: Interrupción en STATE_SAD

Condiciones iniciales: El micro:bit está en el estado STATE_SAD.

Eventos generados: Se presiona el botón A mientras Image.SAD está en pantalla.

Resultados esperados:

La imagen cambia inmediatamente a Image.SMILE.

Image.SMILE se muestra durante 1 segundo.

Luego, el ciclo sigue con Image.SAD por 2 segundos.

Resultados obtenidos: El cambio a Image.SMILE ocurre instantáneamente, y el ciclo sigue como se espera.

✅ Prueba superada.

## Conclusión

El programa logra manejar la concurrencia entre la secuencia de imágenes y la respuesta a la pulsación del botón A sin interrupciones inesperadas. Todos los vectores de prueba confirmaron el correcto funcionamiento del sistema, validando que la implementación de la máquina de estados es efectiva y robusta.
