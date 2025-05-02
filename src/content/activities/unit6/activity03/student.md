# Reflexiones y Experimentos

## 1. Dependencias: las herramientas necesarias

**Reflexión:**  
Usar librerías y módulos como express, http, socket.io y path es muy útil porque estas herramientas ya están optimizadas y son ampliamente utilizadas por la comunidad. Esto ahorra tiempo, pues no necesitas reinventar la rueda. Además, las librerías son mantenidas y actualizadas, lo que asegura que el código que escribas estará basado en buenas prácticas y será más fácil de mantener.

## 2. Creación del Servidor y Socket.IO

**Reflexión:**  
La combinación de express para manejar las peticiones HTTP y socket.io para las conexiones en tiempo real permite crear aplicaciones interactivas y dinámicas de forma sencilla. Express maneja la parte de las solicitudes HTTP de manera eficiente, mientras que Socket.IO facilita la comunicación en tiempo real, algo esencial en aplicaciones de chat o colaboración en línea.

## 3. Variables para guardar el estado

**Reflexión:**  
Usar variables globales como `page1` y `page2` para almacenar información sobre las posiciones y tamaños de las ventanas es clave para permitir la sincronización entre los diferentes clientes conectados al servidor. De esta manera, cuando un cliente actualiza el estado, este se mantiene sincronizado con el servidor y se puede propagar a otros clientes.

## 4. Sirviendo los archivos del cliente

**Experimento:**  
Al cambiar la ruta a una carpeta no existente (`archivos_cliente`), el servidor no encuentra los archivos estáticos y muestra un error en la consola del navegador, indicando que no se puede acceder a esos archivos. Este comportamiento se debe a que Express busca los archivos en la carpeta especificada y, al no encontrarla, no puede servir esos recursos. Volver a la carpeta `views` hace que todo funcione correctamente.

## 5. Rutas: ¿Qué enviar cuando se pide una URL?

**Experimento:**  
Al cambiar la ruta `/page1` por `/pagina_uno`, la URL de acceso cambia y el servidor devuelve la página correcta solo cuando se usa la nueva ruta. Esto muestra cómo el servidor asocia URLs con respuestas específicas definidas en el código. Cambiar la URL implica cambiar la definición de las rutas en el servidor.

## 6. La Magia de Socket.IO: La Conexión

**Experimento:**  
Al abrir dos pestañas en el navegador, una para `page1` y otra para `page2`, la terminal muestra dos IDs diferentes para las conexiones de los clientes. Cuando cierro una de las pestañas, el servidor muestra el ID de la conexión que se ha desconectado, confirmando que cada cliente tiene un ID único y el servidor es capaz de rastrear las conexiones y desconexiones de manera efectiva.

## 7. Escuchando Mensajes de los Clientes

**Experimento:**  
Al mover la ventana de `page1`, la terminal muestra el evento `win1update` con los datos correspondientes. Cuando muevo `page2`, se registra el evento `win2update` con su propio conjunto de datos.  
Si cambio `socket.broadcast.emit` por `socket.emit`, solo el cliente que envía el mensaje recibe la actualización, y los demás no ven el cambio. Esto confirma que `socket.emit` envía el mensaje solo al cliente que lo genera, mientras que `socket.broadcast.emit` lo envía a todos los demás clientes conectados.

## 8. Poner en marcha el Servidor

**Experimento:**  
Al cambiar el puerto a `3001` y reiniciar el servidor, el mensaje en la consola indica que el servidor está escuchando en el nuevo puerto. Sin embargo, al intentar acceder a `http://localhost:3000/page1`, el servidor no responde porque está escuchando en el puerto 3001. Esto demuestra la importancia de la variable `port` y cómo la función `listen()` asocia el servidor con el puerto especificado.

## Conclusión

Durante esta actividad, pude experimentar con los aspectos esenciales de un servidor Node.js utilizando Express y Socket.IO. La capacidad de servir archivos estáticos, definir rutas específicas, gestionar eventos de conexión y desconexión, y actualizar el estado de la aplicación en tiempo real me ha permitido comprender cómo crear una arquitectura básica de servidor interactivo.
