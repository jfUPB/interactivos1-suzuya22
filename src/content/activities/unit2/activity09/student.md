## Codigo semaforo 
let estadoActual = "rojo";  // Estado inicial del semáforo
let tiempoInicio = 0;       // Tiempo de inicio
let duracionRojo = 3000;    // Duración del rojo en milisegundos (3 segundos)
let duracionAmarillo = 1000; // Duración del amarillo en milisegundos (1 segundo)
let duracionVerde = 2000;   // Duración del verde en milisegundos (2 segundos)

function setup() {
  createCanvas(600, 600);  // Creamos una ventana de 400x400 píxeles
  tiempoInicio = millis();  // Guardamos el tiempo inicial
}

function draw() {
  background(0);  // Limpiamos el fondo en cada frame

  // Dependiendo del estado actual, dibujamos el semáforo
  if (estadoActual === "rojo") {
    mostrarSemaforo("rojo");
    if (millis() - tiempoInicio > duracionRojo) {
      estadoActual = "amarillo";
      tiempoInicio = millis();  // Reiniciamos el tiempo al cambiar de estado
    }
  } else if (estadoActual === "amarillo") {
    mostrarSemaforo("amarillo");
    if (millis() - tiempoInicio > duracionAmarillo) {
      estadoActual = "verde";
      tiempoInicio = millis();  // Reiniciamos el tiempo al cambiar de estado
    }
  } else if (estadoActual === "verde") {
    mostrarSemaforo("verde");
    if (millis() - tiempoInicio > duracionVerde) {
      estadoActual = "rojo";
      tiempoInicio = millis();  // Reiniciamos el tiempo al cambiar de estado
    }
  }
}

### Estado rojo
Evento que se evalúa: Se evalúa si ha transcurrido el tiempo de duración del estado "rojo".  
Condición para el evento: Si el tiempo actual (millis() - tiempoInicio) es mayor que duracionRojo (3000 ms o 3 segundos).  
Acción que se ejecuta:  
  Se cambia el estado de estadoActual a "amarillo".  
  Se reinicia el tiempo (tiempoInicio = millis()) para iniciar el conteo para el siguiente estado.  
### Estado amarillo    
Evento que se evalúa: Se evalúa si ha transcurrido el tiempo de duración del estado "amarillo".  
Condición para el evento: Si el tiempo actual (millis() - tiempoInicio) es mayor que duracionAmarillo (1000 ms o 1 segundo).  
Acción que se ejecuta:  
Se cambia el estado de estadoActual a "verde".  
Se reinicia el tiempo (tiempoInicio = millis()) para iniciar el conteo para el siguiente estado.  
### Estado verde  
Acción que se ejecuta:  
Se cambia el estado de estadoActual a "rojo".  
Se reinicia el tiempo (tiempoInicio = millis()) para iniciar el conteo para el siguiente ciclo de cambio de estado.  
