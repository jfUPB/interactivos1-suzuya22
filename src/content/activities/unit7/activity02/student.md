# **Bitácora - Explicación de los Conceptos Clave en la Comunicación entre el Celular y el Computador**

## **1. ¿Por qué es necesario Dev Tunnels en este escenario y cómo funciona conceptualmente?**
Dev Tunnels es necesario en este escenario porque, cuando ejecutamos `npm start`, el servidor está disponible únicamente en `localhost:3000`, que solo es accesible desde el propio computador donde está corriendo el servidor. Si intentáramos acceder a esta URL desde el celular, este buscaría un servidor en su propia máquina, no en el computador. Aunque podríamos usar la IP local de la máquina (`192.168.X.X`), esto solo funcionaría si ambos dispositivos están en la misma red Wi-Fi.

Dev Tunnels actúa como un intermediario que crea un túnel entre el servidor local y una URL pública en Internet. La URL pública se convierte en un punto de acceso accesible desde cualquier lugar. Cuando el celular se conecta a esta URL, el servicio de Dev Tunnels redirige la conexión de manera segura hacia el puerto del servidor local, permitiendo que el celular y el computador se comuniquen.

**Analogía**: Es como tener un número de teléfono público que redirige las llamadas a tu teléfono privado sin exponer directamente tu número privado.

## **2. ¿Por qué usamos JSON.stringify() en el emisor (móvil) y JSON.parse() en el receptor (escritorio)? ¿Qué problema resuelve JSON aquí?**
El motivo por el cual utilizamos `JSON.stringify()` en el emisor (móvil) y `JSON.parse()` en el receptor (escritorio) es que los datos deben ser enviados como cadenas de texto (strings) a través de la red, ya que los protocolos de comunicación no permiten transmitir objetos JavaScript directamente. 

- **`JSON.stringify()`** convierte un objeto JavaScript (como `{ type: 'touch', x: 120, y: 250 }`) en una cadena de texto que puede ser enviada por la red. 
- **`JSON.parse()`** se utiliza en el receptor para convertir la cadena JSON de vuelta en un objeto JavaScript utilizable.

Esto resuelve el problema de enviar datos complejos (como las coordenadas de un toque) de manera eficiente y estructurada, sin perder la integridad de la información. Además, permite que los datos sean fácilmente intercambiables entre diferentes lenguajes y plataformas que entienden el formato JSON.

## **3. Describe la función de `touchMoved()` y por qué se usa la variable `threshold` en el cliente móvil.**
La función `touchMoved()` en p5.js es llamada repetidamente mientras el usuario mantiene el dedo presionado y lo mueve sobre la pantalla. Dentro de esta función, las variables `mouseX` y `mouseY` contienen las coordenadas del toque, lo que permite saber en tiempo real la posición del dedo en la pantalla.

La variable `threshold` se utiliza para evitar que se envíen mensajes cada vez que el dedo se mueve un poco, lo cual podría generar una gran cantidad de datos innecesarios. El código solo envía las coordenadas del toque si el movimiento supera un umbral predefinido (`threshold`), asegurando que solo los movimientos significativos se transmitan. Esto ayuda a reducir la latencia y la carga en la red, mejorando el rendimiento general.

## **4. ¿Qué otros eventos táctiles existen en p5.js y para qué tipo de interacciones podrían ser útiles (ej. un botón virtual, detectar un tap)?**
En p5.js existen otros eventos táctiles útiles para interactuar con la pantalla táctil de un dispositivo. Algunos de estos eventos incluyen:

- **`touchStarted()`**: Se llama cuando el usuario toca la pantalla por primera vez. Puede ser útil para detectar la acción inicial de un usuario, como cuando se presiona un botón virtual.
- **`touchEnded()`**: Se llama cuando el usuario levanta el dedo de la pantalla. Esto podría utilizarse para finalizar una acción, como soltar un objeto o confirmar una selección.
- **`touchMoved()`**: Ya explicado, se utiliza para detectar el movimiento del dedo.
- **`touches[]`**: Esta función proporciona información sobre todos los toques activos en la pantalla. Esto es útil cuando se detectan múltiples toques (por ejemplo, en un juego de dos jugadores).

Estos eventos podrían ser útiles en aplicaciones donde se requiera un control preciso del dedo, como juegos móviles o interfaces de usuario con botones virtuales.

## **5. Compara brevemente Dev Tunnels con simplemente usar la IP local. ¿Cuáles son las ventajas y desventajas de cada uno?**
### **Ventajas de usar Dev Tunnels**:
- **Accesibilidad global**: Permite que el servidor sea accesible desde cualquier lugar, incluso si el celular está fuera de la red local, o si está usando datos móviles en lugar de Wi-Fi.
- **Seguridad**: Dev Tunnels proporciona un túnel seguro, evitando exponer directamente la IP local del servidor a posibles amenazas externas.
- **Facilidad de configuración**: No necesitas configurar redes o firewalls. Solo creas el túnel desde VS Code.

### **Desventajas de usar Dev Tunnels**:
- **Dependencia de una herramienta externa**: Si Dev Tunnels experimenta problemas o es inaccesible, no podrás usar el túnel.
- **Posible latencia**: Dependiendo de la calidad de la conexión a Internet, el túnel puede introducir algo de latencia en la comunicación.

### **Ventajas de usar la IP local**:
- **Conexión directa y rápida**: No depende de servicios externos. Si ambos dispositivos están en la misma red, la comunicación es más rápida.
- **Control total**: Tienes control completo sobre la red y la configuración del servidor.

### **Desventajas de usar la IP local**:
- **Limitación a la misma red**: Solo funciona si ambos dispositivos están conectados a la misma red local, lo que limita su uso a entornos específicos.
- **Problemas de seguridad**: Exponer la IP local puede ser riesgoso, especialmente si no se toman medidas adecuadas para protegerla.

## **Conclusión**
Esta unidad me ayudó a entender cómo manejar la comunicación entre dispositivos usando herramientas como Dev Tunnels y protocolos como JSON. También aprendí sobre los eventos táctiles en p5.js y cómo optimizar la comunicación enviando solo los cambios significativos en los movimientos táctiles.
