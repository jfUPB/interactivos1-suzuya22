## Repaso del caso de estudio
 Comunicación entre el micro:bit y el sketch de p5.js
El micro:bit y el sketch de p5.js se comunican a través de una conexión serial UART, utilizando la biblioteca p5.webserial.js. En el código del micro:bit, se inicializa la UART a 115200 baudios, y cada 100 ms (es decir, a 10 Hz) se envía una línea de texto con los valores del acelerómetro y los estados de los botones A y B:

```python

data = "{},{},{},{}\n".format(xValue, yValue, aState,bState)
uart.write(data)
```
Este texto se envía como cadena ASCII, es decir, los datos son convertidos a texto y separados por comas, con un salto de línea al final para marcar el final de la lectura.

🔤 Estructura del protocolo ASCII
El formato de los datos enviados por el micro:bit es el siguiente:


xValue,yValue,aState,bState\n
Por ejemplo:

```graphql

-230,450,True,False\n
xValue e yValue: valores del acelerómetro en los ejes X e Y.

aState y bState: valores booleanos (True o False) que indican si los botones A o B están siendo presionados.
```
Este protocolo es muy simple pero efectivo: basado en texto plano, con separación por comas, y un \n (salto de línea) para indicar el final de cada paquete de datos.

##  Lectura y transformación de datos en p5.js
La lectura de los datos seriales en p5.js se realiza dentro del draw() loop, mediante este fragmento:

```javascript

if (port.availableBytes() > 0) {
  let data = port.readUntil("\n");
  if (data) {
    data = data.trim();
    let values = data.split(",");
    if (values.length == 4) {
      microBitX = int(values[0]) + windowWidth / 2;
      microBitY = int(values[1]) + windowHeight / 2;
      microBitAState = values[2].toLowerCase() === "true";
      microBitBState = values[3].toLowerCase() === "true";
      updateButtonStates(microBitAState, microBitBState);
    }
  }
}
```
Se lee la línea completa con readUntil("\n").

Se eliminan espacios innecesarios con trim().

Se separa en cuatro valores con split(",").

Los valores X e Y se convierten a enteros y se transforman para que estén centrados en la ventana (+ windowWidth/2 y + windowHeight/2).

Los estados de los botones se convierten a valores booleanos (true o false) usando comparación con "true".

##  Generación de eventos A pressed y B released
Esto se maneja dentro de la función updateButtonStates:

```javascript

if (newAState === true && prevmicroBitAState === false) {
  // Evento A PRESSED
  lineModuleSize = random(50, 160);
  clickPosX = microBitX;
  clickPosY = microBitY;
  print("A pressed");
}

if (newBState === false && prevmicroBitBState === true) {
  // Evento B RELEASED
  c = color(random(255), random(255), random(255), random(80, 100));
  print("B released");
}
```
A pressed se genera solo cuando se detecta una transición de false a true en el botón A.

B released ocurre cuando el botón B pasa de true a false.

Esto funciona porque se mantiene un estado anterior (prevmicroBitAState, prevmicroBitBState) y se compara con el nuevo valor en cada frame.

