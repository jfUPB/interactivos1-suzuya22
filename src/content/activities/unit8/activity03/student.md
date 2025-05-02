# 🧠 Actividad 03 – Proyecto I-P-O: Definición Conceptual

## 🌩️ Brainstorming Integrador – 3 Ideas Iniciales

### 💡 Idea 1: Jardín emocional interactivo
- **Narrativa:** Un paisaje que refleja el estado emocional del usuario. El micro:bit capta el nivel de movimiento (ansiedad o calma), mientras que el móvil envía mensajes de ánimo o presión.
- **Inputs:**
  - Micro:bit (acelerómetro): mide nivel de movimiento (agitación).
  - Móvil (toques o pulsaciones): actúan como interrupciones o influencias externas.
- **Proceso:** Si el micro:bit detecta mucha agitación, el jardín se vuelve oscuro y tormentoso. Si el móvil envía una vibración o toque suave, el entorno intenta calmarse.
- **Output:** Un paisaje animado que se transforma dinámicamente.

---

### 💡 Idea 2: Escultura sonora colectiva
- **Narrativa:** Varias personas contribuyen a una escultura de sonido y visual, donde los datos del micro:bit y el móvil modifican sus formas.
- **Inputs:**
  - Micro:bit (botón A/B): cambia de forma o añade capas.
  - Móvil (acelerómetro o inclinación): cambia el ritmo o la textura del sonido.
- **Proceso:** Cada input transforma un parámetro de la escultura, sumando elementos a tiempo real.
- **Output:** Visualización 3D o abstracta + sonido modificado en tiempo real.

---

### 💡 Idea 3: Carrera emocional de avatares
- **Narrativa:** Dos avatares corren por una pista que representa el estado emocional y físico del usuario. El micro:bit mide calma física, y el móvil representa distracciones digitales.
- **Inputs:**
  - Micro:bit (nivel de quietud): mientras más quieto, el avatar avanza.
  - Móvil (toques): cada toque representa una distracción y ralentiza el avance.
- **Proceso:** El sketch evalúa el equilibrio entre foco (quietud) y distracción.
- **Output:** Carrera visual entre dos personajes con efectos gráficos y medallas según el rendimiento.

---

## ⭐ Concepto I-P-O Final

### 🎮 Título Provisional: *Avatar Mind Balance*

---

### 📖 Narrativa/Concepto Central
Un avatar recorre un mundo mental donde el equilibrio entre calma y distracción determina el avance. El usuario debe mantenerse quieto (meditación) para que el avatar progrese, pero cada interacción desde el móvil representa una interrupción que lo frena o desvía. La visualización busca hacer reflexionar sobre el foco y el entorno digital.

---

### 🔌 Inputs Específicos

| Fuente     | Dispositivo  | Variable        | Justificación |
|------------|--------------|-----------------|----------------|
| Micro:bit  | Acelerómetro | Nivel de movimiento (X/Y/Z promedio o magnitud) | Representa el estado físico/mental del usuario. Más quietud = más foco. |
| Móvil      | Socket.IO    | Toques/clicks   | Representan interrupciones o distracciones externas. |
| Computador | Teclado/mouse | Opcional (pausar/reiniciar) | Control local del sketch por el usuario. |

---

### 🧠 Process – Lógica en p5.js (Cliente)

- **Recepción de Datos:**
  - Desde Serial (micro:bit): Se recibe el valor de aceleración.
  - Desde Socket.IO (móvil): Se reciben eventos de toque o clic.

- **Transformación:**
  - Si la aceleración es baja por cierto tiempo, `foco++` y el avatar avanza.
  - Si hay una señal del móvil, `distracción++` y el avatar se ralentiza o se desvía.
  - Variables de estado clave:
    - `nivelFoco` (int)
    - `nivelDistraccion` (int)
    - `posAvatar` (float)
    - `estado` (calmo, distraído, equilibrado)

- **Reglas:**
  - Cada segundo de quietud suma 1 punto de foco.
  - Cada toque en el móvil resta 2 puntos o genera un obstáculo en la ruta.
  - El balance determina la velocidad o dirección del avatar.

---

### 🎨 Output Deseado (p5.js)

- **Visualización:**
  - Un avatar recorriendo un mundo abstracto.
  - Elementos visuales que cambian según el estado (`foco`, `distracción`).
  - Obstáculos/distractores generados por el móvil.
  - Progreso mostrado en una barra o medalla animada.

- **Interacción:**
  - El usuario puede observar cómo su comportamiento físico y digital afecta el recorrido.
  - Una opción para pausar/reiniciar la experiencia.

---

### ✍️ Boceto (descripción visual)

