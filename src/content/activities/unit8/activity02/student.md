# Análisis I-P-O de Ejemplos Inspiradores

## ✨ Ejemplo 1: *Philharmonie Luxembourg* – Patrik Hübner

### **Input**
- **Tipo de datos:** Interacción humana mediante el desplazamiento por el sitio web (scroll), clics, y visualización del contenido.
- **Fuentes:** Navegador web (comportamiento del usuario), posiblemente también tiempo o fecha.
- **Múltiples inputs:** Sí; se combinan datos del scroll, clics y contexto visual.

### **Process**
- La lógica procesa el comportamiento del usuario para decidir qué animaciones y transformaciones se deben activar.
- La experiencia se adapta dinámicamente dependiendo del ritmo de navegación del usuario.
- Este procesamiento probablemente ocurre en el cliente (navegador) usando JavaScript para respuestas inmediatas.

### **Output**
- **Resultado visual:** Transiciones suaves, transformaciones gráficas, animaciones sincronizadas con el desplazamiento.
- **Tiempo real:** Sí, la visualización responde inmediatamente al comportamiento del usuario.

### **Narrativa/Concepto**
Explora cómo la arquitectura y el diseño pueden ser vividos digitalmente a través de una narrativa visual interactiva, dando al usuario la sensación de recorrer físicamente un espacio musical.

---

## ✨ Ejemplo 2: *Brute* – Estudio creativo

### **Input**
- **Tipo de datos:** Parámetros tipográficos generados a partir de la interacción humana o valores predeterminados.
- **Fuentes:** Usuario elige palabras, estilos, intensidades de distorsión (posiblemente a través de sliders o controles interactivos).
- **Múltiples inputs:** Sí, combina selección textual + parámetros de estilo y distorsión.

### **Process**
- Algoritmos transforman la forma de las letras según los parámetros definidos.
- Procesamiento gráfico que modifica geometrías tipográficas y genera una identidad visual única para cada palabra.
- Probablemente ocurre en el cliente (navegador) usando WebGL, p5.js o una librería similar.

### **Output**
- **Resultado visual:** Tipografía dinámica y visualmente distorsionada.
- **Tiempo real:** Sí, los cambios aparecen conforme el usuario modifica los parámetros.

### **Narrativa/Concepto**
Una herramienta que convierte palabras en experiencias visuales únicas, destacando cómo el significado puede alterarse a través de la forma y el movimiento.

---

## 💡 Reflexión para mi Proyecto Final

Estos ejemplos me inspiran a:
- **Combinar múltiples fuentes de input**, como los datos del micro:bit y el móvil, para generar una experiencia interactiva rica y dinámica.
- **Centrarme en una narrativa visual clara**, donde cada interacción tenga un impacto comprensible y estéticamente interesante.
- Usar **procesamiento local en p5.js** para mantener una respuesta en tiempo real.
- Implementar **outputs visuales responsivos** que reflejen claramente los cambios generados por el input (como en *Philharmonie*), o incluso outputs más abstractos que expresen un concepto, como en *Brute*.

Quiero explorar una narrativa en la que el micro:bit represente el “estado emocional” del sistema y el móvil actúe como un “control externo” o “intención”, y así el cliente p5.js interprete ambas cosas para generar una visualización viva, sensible y reactiva.
