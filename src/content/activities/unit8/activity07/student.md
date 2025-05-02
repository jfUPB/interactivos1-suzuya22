# 🛰️ Proyecto Final Interactivo – Curso Interacción Físico-Digital

## 🧩 Título del Proyecto
**"EmoSphere: Esfera Emocional Interactiva"**

## 📝 Resumen
Este proyecto propone una esfera visual en p5.js que representa el estado emocional de un personaje ficticio. Recibe datos de sensores de movimiento y botones de un micro:bit, interacciones táctiles desde un móvil, y entradas desde teclado o mouse del usuario en el computador. Estos inputs se procesan y combinan en tiempo real, generando una visualización interactiva que reacciona con color, tamaño y estado textual. La arquitectura sigue el modelo I-P-O (Input – Process – Output) e integra múltiples plataformas mediante un servidor puente usando Socket.IO y comunicación serial.

---

## 📊 Concepto I-P-O

### 🧠 Narrativa
La esfera refleja el "estado emocional" de un personaje invisible. A mayor interacción y movimiento, más "feliz" y activa. La falta de interacción la vuelve "neutral" o incluso "estresada". Es un espejo emocional del entorno físico-digital.

### 🔽 Inputs

| Fuente      | Input                                 |
|-------------|----------------------------------------|
| Micro:bit   | Acelerómetro eje Z, Botón A            |
| Móvil       | Toques detectados y enviados por Socket |
| PC local    | Presión de la tecla espacio            |

### ⚙️ Process (Implementado en `draw()` y funciones en p5.js)

- El eje Z del acelerómetro controla el nivel de actividad.
- El botón A disminuye la energía (relajación).
- Los toques móviles aumentan la energía (motivación).
- La tecla espacio cambia el color base del personaje.
- Se aplican límites y reglas de cambio de estado según umbrales.

Estados posibles: `"neutral"`, `"feliz"`, `"estresado"`, `"relajado"`.

### 🖥️ Output

- Cambios de color del canvas y esfera central.
- Tamaño dinámico de la esfera.
- Texto indicativo del estado emocional.
- (Opcional) Sonido o vibración si se desea agregar en móvil.

---

## 🧱 Diseño Técnico: Arquitectura Final

### 📐 Diagrama (Representación textual aquí; puede adjuntarse imagen)

Micro:bit --> Serial --> Servidor Node.js <-- Socket.io --> Móvil
|
+--> Cliente p5.js (visualización)

markdown
Copiar
Editar

### 🔧 Tecnologías y Protocolos

- **Hardware**: Micro:bit v2
- **Lenguajes**: JavaScript (p5.js, Node.js)
- **Protocolos**: WebSocket (Socket.IO), Serial (p5.serialport)
- **Entorno**: Navegador Web para p5.js, Terminal Node.js

---

## 🔧 Implementación

### Componentes

#### 🧲 Micro:bit
- Envia datos por puerto serial: `x,y,z,botonA,botonB`
- Código en MakeCode o Python

#### 📱 Móvil
- Página web móvil con interfaz táctil
- Envía eventos `tap` mediante `socket.emit()` al servidor puente

#### 🌉 Servidor puente (Node.js)
- Recibe datos del micro:bit y del móvil
- Reenvía eventos al cliente p5.js

#### 🧠 Cliente p5.js
- Recibe datos en tiempo real desde el servidor
- Lógica de procesamiento implementada en `draw()` y `procesarEstado()`
- Renderiza visualización dinámica

### ▶️ Instrucciones para ejecutar

1. Conectar micro:bit vía USB a la computadora.
2. Abrir terminal y ejecutar el servidor:
   ```bash
   node server.js
Abrir el archivo index.html del cliente p5.js en navegador de escritorio.

Desde un móvil conectado a la misma red, abrir la interfaz móvil (si aplica).

Interactuar: mover micro:bit, tocar móvil, presionar espacio.

✅ Confirmado: las instrucciones han sido probadas con éxito.

✅ Resultados
Sistema funcional de múltiples entradas (micro:bit + móvil + PC).

Output dinámico en canvas que cambia en tiempo real.

Interacción intuitiva y estética.

Comunicación estable a través de servidor puente.
