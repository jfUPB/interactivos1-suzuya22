# Bitácora de Estudio: Navegando en el Mundo Digital

## 1. El Gran mapa: ¿Qué es Internet?

### Reflexión:
- **Conexión a Internet**: Yo suelo conectarme a Internet mediante Wi-Fi. Si se corta la rampa de acceso (el Wi-Fi), no puedo acceder a ninguna página web. Mi conexión depende completamente de esta infraestructura.
  
---

## 2. Tu vehículo y tu destino: navegador y servidor

### Reflexión:
- **Ejemplos de Cliente-Servidor en la vida diaria**:
  - Al pedir comida en un restaurante: el cliente (nosotros) pide la comida, y el servidor (el camarero) la lleva. El cliente pide, y el servidor responde.
  - Al hacer una llamada telefónica: el teléfono (cliente) solicita una llamada, y la central telefónica (servidor) conecta la llamada.

---

## 3. La dirección exacta: ¿Qué es una URL?

### Reflexión:
- **Análisis de una URL**: 
  - **Protocolo**: `http://` es el protocolo de comunicación.
  - **Dominio**: `www.ejemplo.com` es el nombre del servidor.
  - **Ruta**: `/pagina/index.html` es el recurso dentro del servidor.
- Si solo escribo el dominio (`www.google.com`), el servidor probablemente me redirigirá a la página de inicio (por defecto).

---

## 4. La conversación: protocolo HTTP

### Reflexión:
- **Similitudes y diferencias con protocolos seriales**:
  - En ambos casos, hay una solicitud y una respuesta (como con micro:bit y p5.js).
  - La diferencia clave es que HTTP maneja más detalles como encabezados, códigos de estado y diferentes métodos de solicitud (GET, POST), lo que es esencial para una comunicación web más compleja.

---

## 5. El Paquete recibido: HTML, CSS y JavaScript

### Reflexión:
- **Página web de login**:
  - **HTML**: Los campos de texto y el botón.
  - **CSS**: El color del botón y la tipografía.
  - **JavaScript**: La validación de los datos antes de enviar el formulario y el mensaje de error si la contraseña es incorrecta.

---

## 6. ¡Despierta, JavaScript! ¿Cómo se Ejecuta?

### Reflexión:
- **Ejecución de JavaScript**: El código JavaScript puede ejecutarse de forma inmediata o diferida (después de cargar la página). Esto es importante para asegurar que el DOM esté completamente disponible antes de que el código JavaScript comience a interactuar con él.

---

## 7. El Modelo de ejecución: imperativo vs basado en eventos

### Reflexión:
- **Ventajas del modelo basado en eventos**:
  - El modelo basado en eventos es eficiente porque solo se ejecutan funciones cuando ocurren eventos específicos (como hacer clic en un botón), en lugar de un bucle constante como en el modelo imperativo de `p5.js`.
  - Esto evita que el navegador esté ejecutando código innecesariamente cuando nada ha cambiado.

---

## 8. El mago detrás del telón: ¿Qué es Node.js?

### Reflexión:
- **Ventajas de usar JavaScript en el cliente y el servidor**:
  - Usar JavaScript tanto en el navegador como en el servidor permite que los desarrolladores usen un solo lenguaje en todo el proyecto, lo que puede hacer el desarrollo más eficiente y simplificado.

---

## 9. WebSockets y Socket.IO

### Reflexión:
- **Diferencia entre HTTP y WebSockets/Socket.IO**:
  - **HTTP**: Petición/Respuesta, similar a enviar un correo electrónico, donde se espera una respuesta.
  - **WebSockets/Socket.IO**: Comunicación instantánea y bidireccional, como una llamada telefónica en la que ambas partes pueden hablar al mismo tiempo sin necesidad de nuevos mensajes de solicitud.
  - **Aplicaciones en tiempo real**: Chat en vivo, juegos en línea, colaboraciones en tiempo real, seguimiento de posición en aplicaciones de mapas.

---
