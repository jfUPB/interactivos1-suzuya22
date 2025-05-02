## ¿Qué es un protocolo de comunicación y por qué es importante en la comunicación serial?

Un protocolo de comunicación es un conjunto de reglas que define cómo se debe intercambiar la información entre dos sistemas. En la comunicación serial, un protocolo es crucial para asegurar que los datos se envíen de manera ordenada y comprensible entre dispositivos. Esto implica la definición de cómo se estructuran los datos, cómo se sincronizan y cómo se gestionan los errores, entre otros. Sin un protocolo, los dispositivos no entenderían cómo interpretar los datos que se envían.

Fragmento de código:
``` javascript

// Se utiliza un protocolo para enviar los datos del micro:bit como una cadena delimitada por comas
let data = xValue + "," + yValue + "," + aState + "," + bState;
serial.write(data + "\n");
```
Aquí, los datos son estructurados en una cadena con valores separados por comas, lo que sigue un formato específico que ambos dispositivos (el micro:bit y la aplicación p5.js) pueden entender.

## ¿Por qué se separan los datos con comas en el protocolo ASCII que exploramos?

Los datos se separan con comas para facilitar su procesamiento y separación en distintas partes. El uso de comas como delimitadores es una forma eficiente de estructurar la información y permite que el receptor pueda dividir la cadena y procesar cada valor de forma independiente.

Fragmento de código:

``` javascript

let values = split(data, ',');
```
Aquí, la función split() usa la coma como delimitador para dividir la cadena recibida en distintas partes (por ejemplo, xValue, yValue, aState, bState).

## ¿Por qué es necesario terminar los datos con un carácter que marque el fin del mensaje?

Es necesario un carácter de fin de mensaje para indicar que los datos completos han sido enviados y que no hay más información disponible. Sin este carácter, el receptor no sabría cuándo ha terminado el mensaje y podría leer datos incompletos o incorrectos.

Fragmento de código:

```javascript

serial.write(data + "\n");
````
El carácter de salto de línea ("\n") se utiliza para marcar el final del mensaje, asegurando que el receptor entienda que todos los datos han sido enviados.

## ¿Por qué fue necesario usar una máquina de estados en la aplicación modificada de p5.js?

Una máquina de estados se utiliza para controlar el flujo de la aplicación y manejar diferentes condiciones de manera estructurada. En este caso, la máquina de estados es útil para gestionar los diferentes estados de los botones (A y B) y asegurar que la aplicación reaccione correctamente a las entradas del micro:bit en función de su estado.

## ¿Cómo se formatean los datos en el micro:bit para ser enviados por el puerto serial?

En el micro:bit, los datos se formatean en cadenas de texto separadas por comas, siguiendo el protocolo definido. Cada valor (por ejemplo, xValue, yValue, aState, bState) se convierte en un número o una cadena y se envía como parte de una única línea.

Fragmento de código en micro:bit (pseudocódigo):

```javascript

let data = xValue + "," + yValue + "," + aState + "," + bState;
serial.write(data + "\n");
```
## ¿Qué significa que los datos enviados por el micro:bit están codificados en ASCII?

Los datos codificados en ASCII significan que cada carácter en la cadena se convierte en un valor numérico según el estándar ASCII. Por ejemplo, el carácter '1' tiene el valor ASCII 49, y el carácter 'True' se envía como una cadena de caracteres.

## ¿Por qué es necesario en la aplicación de p5.js preguntar si hay bytes disponibles en el puerto serial antes de leerlos?

Es necesario preguntar si hay bytes disponibles para evitar leer datos incompletos o vacíos, lo cual podría causar errores en el procesamiento. Al verificar la disponibilidad de datos, nos aseguramos de que solo leemos cuando haya información que procesar.

Fragmento de código:

``` javascript

if (port.availableBytes() > 0) {
  let data = port.readUntil("\n");
}
```
## ¿Qué pasa si esto no se hace?

Si no se verifica la disponibilidad de datos, el programa podría intentar leer datos cuando no hay información disponible, lo que resultaría en un error o en la lectura de datos vacíos o incompletos.

## ¿Cómo se elimina el retorno de carro o salto de línea de un string en p5.js?

El retorno de carro o salto de línea se puede eliminar utilizando la función trim() en p5.js, que elimina cualquier espacio o salto de línea al principio y al final de la cadena.

Fragmento de código:

``` javascript

let cleanData = data.trim();
```
Si una cadena tiene información separada por espacios y quieres dividir dicha información en varias cadenas individuales ¿Qué función de p5.js usarías?

La función split() se utiliza para dividir una cadena en varias subcadenas según el delimitador que se especifique, en este caso, el espacio.

Fragmento de código:

```javascript

let values = split(data, ' ');
```
## ¿Por qué es necesario en la aplicación del caso de estudio convertir las cadenas a números enteros antes de usarlas en el sketch de p5.js?

Los valores enviados por el micro:bit son cadenas de texto, pero necesitamos trabajar con valores numéricos en p5.js para realizar cálculos, como la manipulación de tamaños y colores de los círculos. Convertir las cadenas a números enteros asegura que se puedan usar en operaciones matemáticas.

Fragmento de código:

```javascript

xValue = int(values[0]);
yValue = int(values[1]);
```
Si el micro:bit tiene los siguientes datos xValue: 123, yValue: 756, aState: False, bState: True ¿Qué bytes se enviarían por el puerto serial?

Los datos enviados serían: '123,756,False,True\n'. El valor xValue y yValue son enteros, y aState y bState son valores booleanos representados como las cadenas 'False' y 'True'. Todo esto se enviaría como una cadena de texto codificada en ASCII.

¿Qué aprendiste nuevo del micro:bit que no sabías antes?

Aprendí que el micro:bit puede enviar datos a través del puerto serial, y estos datos pueden ser codificados en ASCII. También descubrí cómo usar el micro:bit para obtener valores del acelerómetro y los botones, y transmitir esta información a una aplicación externa.

¿Qué aprendí nuevo de p5.js que no sabías antes?

Aprendí a usar la API de p5.js para leer datos del puerto serial mediante el uso de la librería p5.SerialPort. Además, descubrí cómo trabajar con la función split() para dividir cadenas y cómo mapear valores para crear efectos visuales basados en datos dinámicos.
