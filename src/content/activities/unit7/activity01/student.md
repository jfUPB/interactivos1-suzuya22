# **Actividad 01: Observa funcionando el caso de estudio**

## **URL de Dev Tunnels obtenida**
La URL de Dev Tunnels que obtuve fue: `https://mi-url-ejemplo.usawdawde2.devtunnels.ms/`. Necesitamos usar esta URL en lugar de `http://localhost:3000` o la IP local de mi computador porque el túnel expone el servidor local a una dirección accesible desde cualquier lugar, permitiendo que dispositivos fuera de la red local, como mi celular, se conecten a la aplicación.

## **¿Qué hace `npm install` y `npm start`?**
- **`npm install`**: Este comando instala todas las dependencias necesarias para el proyecto, como `express` y `socket.io`, basándose en las especificaciones del archivo `package.json`. Lo usamos para asegurarnos de que todas las bibliotecas requeridas estén disponibles para que el servidor funcione correctamente.
- **`npm start`**: Ejecuta el servidor local. Este comando inicia el servidor en el puerto `3000` y permite que la aplicación esté disponible en esa URL. De esta forma, se comienza a escuchar las solicitudes del cliente, y el servidor se mantiene activo para manejar las conexiones entrantes.

## **Mensajes observados en la terminal del servidor**
Al conectar el cliente de escritorio y el cliente móvil, los mensajes que observé en la terminal del servidor fueron:
- **"New client connected"**: Este mensaje apareció cuando un cliente (ya sea el de escritorio o móvil) se conectó al servidor.
- **"Received message => ..."**: Este mensaje se mostró cada vez que el cliente móvil enviaba las coordenadas del toque para mover el círculo. El servidor recibía estas coordenadas y las transmitía a todos los clientes conectados.
- **"Client disconnected"**: Este mensaje apareció cuando cerré una de las pestañas del navegador, indicando que el cliente se desconectó.

Los mensajes fueron similares para ambos clientes. El identificador del cliente fue único, lo que permitió al servidor distinguir entre ellos, pero el proceso de conexión y desconexión fue el mismo.

## **Comportamiento observado**
La interacción funcionó correctamente. El círculo en el navegador de mi computadora se movió en tiempo real siguiendo mi dedo sobre la pantalla del celular. La latencia fue mínima, y no noté retrasos significativos. Sin embargo, en algunos momentos hubo una ligera demora al mover el dedo rápidamente sobre la pantalla, pero nada que afectara la experiencia de usuario.

## **¿Cerraste el puerto?**
Sí, cerré el puerto después de completar las pruebas. Es muy importante cerrar el puerto para evitar riesgos de seguridad, ya que tener puertos abiertos innecesariamente puede exponer el sistema a vulnerabilidades. Además, al cerrar el puerto, se liberan los recursos y el servidor ya no queda accesible desde fuera de la red.

## **Reflexión final**
Esta actividad me permitió entender cómo utilizar Dev Tunnels para exponer un servidor local de manera segura a través de una URL pública. Además, me permitió ver cómo funciona la comunicación en tiempo real entre clientes y el servidor usando `socket.io`, lo cual es fundamental para desarrollar aplicaciones interactivas en tiempo real. La herramienta Dev Tunnels fue esencial para poder conectar dispositivos fuera de la red local, y esta experiencia ha mejorado mi comprensión sobre cómo establecer conexiones de este tipo.
