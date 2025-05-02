## Actividad 02 - Caso de estudio: micro:bit y comunicación binaria
##  Objetivo
Transformar el código del micro:bit para que los datos se envíen en formato binario con struct.pack, compararlo con el formato de texto ASCII y analizar sus ventajas y desventajas.

##  🧐🧪✍️ Visualización de datos como texto (ASCII)
Resultado del experimento:
Al usar la opción “Mostrar datos como: Texto” en la aplicación SerialTerminal, los datos enviados en binario aparecen como caracteres extraños, ilegibles o no aparecen en absoluto.

¿Por qué sucede esto?
El formato binario no es legible como texto. La aplicación intenta interpretar bytes crudos como si fueran caracteres ASCII, pero muchos de esos valores no tienen representación textual visible, por lo tanto, se ven como "basura".

##  🧐🧪✍️ Visualización de datos como hexadecimal
Resultado del experimento:
Con la opción “Mostrar datos como: Todo en Hex”, los datos se ven como secuencias de bytes, por ejemplo:
00 F2 01 2C 01 00
Estos valores varían dependiendo de los valores del acelerómetro y los botones.

¿Cómo se relaciona con el código?

```python

data = struct.pack('>2h2B', xValue, yValue, int(aState), int(bState))
```
>2h2B indica:

>: orden de bytes grande (big-endian)

2h: dos enteros cortos de 2 bytes cada uno (x, y)

2B: dos enteros sin signo de 1 byte (botón A, botón B)

Total: 2 + 2 + 1 + 1 = 6 bytes por mensaje

##  🧐🧪✍️ Ventajas y desventajas del formato binario
Ventajas:

Menor tamaño de los datos (más eficiente).

Transmisión más rápida, ideal para tiempo real o alto rendimiento.

Mejor para sensores que emiten muchos datos.

Desventajas:

No es legible para humanos.

Más difícil de depurar sin herramientas especializadas.

Necesita interpretación explícita en el programa receptor.

## 🧪✍️ Experimento con gesto de agitar
Código utilizado:

```python

while True:
    if accelerometer.was_gesture('shake'):
        xValue = accelerometer.get_x()
        yValue = accelerometer.get_y()
        aState = button_a.is_pressed()
        bState = button_b.is_pressed()
        data = struct.pack('>2h2B', xValue, yValue, int(aState), int(bState))
        uart.write(data)
```
¿Cuántos bytes se envían por mensaje?
6 bytes:

2h: 2 × 2 bytes = 4 bytes

2B: 2 × 1 byte = 2 bytes

¿Qué significa cada byte?:

Bytes 1–2: valor x del acelerómetro (entero con signo)

Bytes 3–4: valor y del acelerómetro

Byte 5: botón A (0 o 1)

Byte 6: botón B (0 o 1)

##  🧐✍️ Representación de negativos en binario
Los valores negativos en xValue o yValue se codifican en complemento a dos, como es estándar en enteros con signo. En hexadecimal, por ejemplo:

-1 → FF FF

-256 → FF 00

Esto se debe a que h (short) permite representar enteros de -32768 a 32767.

##  🧪✍️ Comparación entre ASCII y binario
Código del experimento:

```python

data = struct.pack('>2h2B', xValue, yValue, int(aState), int(bState))
uart.write(data)
uart.write("ASCII:\n")
data = "{},{},{},{}\n".format(xValue, yValue, aState,bState)
uart.write(data)
```
Resultado:

Se ve una secuencia binaria seguida por la palabra “ASCII:” y luego la cadena legible, por ejemplo:
00 F2 01 2C 01 00 41 53 43 49 49 3A ...

El texto legible aparece correctamente después de la palabra ASCII:

Diferencias observadas:

Formato	Ventajas	Desventajas
Binario	Más compacto, eficiente, rápido	No legible, más difícil de depurar
ASCII	Legible, fácil de depurar	Más lento, ocupa más espacio

