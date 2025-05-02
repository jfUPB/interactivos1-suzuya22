## Comparación entre Protocolo ASCII y Protocolo Binario

| Aspecto                  | Protocolo ASCII                                 | Protocolo Binario                                |
|--------------------------|-------------------------------------------------|--------------------------------------------------|
| **Eficiencia**            | Menos eficiente debido a que cada carácter ocupa 1 byte más el carácter de control, lo que genera un mayor uso de recursos. | Más eficiente, ya que los datos se representan de manera más compacta (menos bits por carácter). |
| **Velocidad**             | Menos rápido por la mayor cantidad de bytes transmitidos y la necesidad de conversiones. | Más rápido por la menor cantidad de datos a transmitir y la simplicidad de la codificación. |
| **Facilidad**             | Fácil de leer para los humanos, ya que es texto legible. | Más complejo de leer y trabajar, ya que los datos son representados en binario, que no es legible directamente. |
| **Usos de recursos**      | Utiliza más ancho de banda y almacenamiento debido a la codificación más grande. | Usa menos recursos, tanto en términos de espacio como de ancho de banda, debido a la compacidad del binario. |

**Justificación con ejemplos concretos:**
- En una aplicación modificada, como la comunicación de datos de un sensor, el protocolo ASCII podría enviar un número como "1234", mientras que el protocolo binario lo enviaría como un valor de 16 bits, lo que resulta en una transmisión más eficiente en términos de espacio.

## Framing en el Protocolo Binario

- **¿Por qué fue necesario introducir framing en el protocolo binario?**
  El framing es necesario en el protocolo binario para identificar los límites de cada paquete de datos. Dado que los datos en binario no tienen una estructura de texto legible, no hay manera directa de saber cuándo empieza o termina un mensaje, por lo que es necesario usar marcos (framing) para definir claramente los datos válidos.

- **¿Cómo funciona el framing?**
  El framing utiliza delimitadores o caracteres de control (como los bytes de inicio y fin) para identificar el principio y el final de un paquete de datos. Esto asegura que los datos sean procesados correctamente, evitando la confusión entre diferentes paquetes.

- **¿Qué es un carácter de sincronización?**
  Es un byte específico (como `0xaa`) que se utiliza para indicar el comienzo de un paquete de datos. Permite sincronizar la lectura y el procesamiento de los datos.

- **¿Qué es el checksum y para qué sirve?**
  El checksum es una suma de comprobación que se calcula para verificar la integridad de los datos. Si el checksum calculado no coincide con el checksum enviado, significa que los datos pueden haber sido alterados o corrompidos en la transmisión.

## Función `readSerialData()` en p5.js

- **¿Qué hace la función `concat`? ¿Por qué?**
  La función `concat` en el código combina los nuevos datos leídos desde el puerto serie con el contenido previamente almacenado en `serialBuffer`. Esto es necesario para construir un buffer completo de datos antes de procesarlo.

- **¿Por qué se recorre el buffer solo si tiene 8 o más bytes?**
  Se hace para asegurarse de que haya suficientes datos en el buffer para procesar un paquete completo. Si el buffer tiene menos de 8 bytes, no es posible realizar un procesamiento adecuado del paquete.

- **¿Qué significa `0xaa`?**
  `0xaa` es un valor hexadecimal que se usa como un carácter de sincronización al inicio de cada paquete. Indica que el paquete de datos está comenzando.

- **¿Qué hace la función `shift` y la instrucción `continue`? ¿Por qué?**
  La función `shift` elimina el primer elemento del array `serialBuffer` si no coincide con el valor de sincronización `0xaa`. La instrucción `continue` hace que el bucle pase a la siguiente iteración sin procesar el paquete actual, ya que es inválido.

- **Si hay menos de 8 bytes, ¿qué hace la instrucción `break`? ¿Por qué?**
  La instrucción `break` termina el bucle si no hay suficientes bytes (menos de 8) para procesar. Esto evita que el código intente leer un paquete incompleto.

## Diferencia entre `slice` y `splice`

- **¿Cuál es la diferencia entre `slice` y `splice`?**
  - `slice` devuelve una copia superficial de una sección de un arreglo sin modificar el arreglo original.
  - `splice` cambia el arreglo original, eliminando, reemplazando o agregando elementos.
  
  **¿Por qué se usa `splice` justo después de `slice`?**
  Después de usar `slice` para extraer el paquete de datos, se utiliza `splice` para eliminar esos 8 bytes del buffer, asegurando que no se procesen nuevamente.

## Programación Funcional y la Función `reduce`

- **¿Cómo opera la función `reduce`?**
  La función `reduce` toma una función de callback que se aplica de forma acumulativa a cada valor de un arreglo. En el caso del código proporcionado, se calcula la suma de todos los valores de `dataBytes`, y el resultado se reduce a un valor de 8 bits mediante el módulo 256.

- **¿Por qué se compara el checksum enviado con el calculado? ¿Para qué sirve esto?**
  Se compara para verificar si los datos fueron corrompidos en la transmisión. Si los dos valores no coinciden, se sabe que hay un error en los datos recibidos.

- **¿Qué hace la instrucción `continue` en este contexto?**
  Si el checksum calculado no coincide con el checksum recibido, la instrucción `continue` hace que el ciclo ignore ese paquete y pase al siguiente, evitando procesar datos corruptos.

## DataView

- **¿Qué es un `DataView`? ¿Para qué se usa?**
  Un `DataView` es una interfaz que permite leer y escribir valores de tipo específico (por ejemplo, enteros, flotantes) desde un buffer de manera más flexible que un array de bytes. Se usa para interpretar los datos binarios de manera eficiente.

- **¿Por qué es necesario hacer estas conversiones y no simplemente tomar los datos del buffer?**
  Es necesario hacer estas conversiones porque los datos binarios necesitan ser interpretados correctamente según su tipo (por ejemplo, enteros de 16 bits, valores booleanos). El `DataView` proporciona una forma de interpretar esos datos de manera precisa.
