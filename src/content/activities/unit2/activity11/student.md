## Descripción de la Máquina de Estados
Este es un pequeño proyecto que simula una bomba con cuenta regresiva, implementada como una máquina de estados finitos. Básicamente, la bomba tiene tres modos:

### Modo Configuración 

Permite ajustar el tiempo entre 10 y 60 segundos.
Se puede aumentar o disminuir el tiempo con botones.
Al presionar otro botón, pasamos al Modo Cuenta Regresiva.

### Modo Cuenta Regresiva 

El temporizador empieza a descontar segundos.
Si el tiempo llega a cero, pasamos al estado de Explosión.

### Explosión 

Muestra un mensaje indicando que la bomba explotó.

La única forma de salir de este estado es presionando un botón que reinicia la bomba y la regresa a Modo Configuración.

## Codigo pensado para el problema
``` Js

const CONFIGURATION_MODE = "CONFIGURATION_MODE";
const COUNTDOWN_MODE = "COUNTDOWN_MODE";
const EXPLOSION_STATE = "EXPLOSION_STATE";


let estado_actual = CONFIGURATION_MODE; // Estado inicial
let tiempo_restante = 20; // Tiempo por defecto en segundos
let start_time = 0; // Para guardar el momento en que inicia la cuenta regresiva


function aumentarTiempo() {
    if (tiempo_restante < 60) {
        tiempo_restante++;
        console.log(`🕒 Tiempo ajustado: ${tiempo_restante} segundos`);
    }
}


function disminuirTiempo() {
    if (tiempo_restante > 10) {
        tiempo_restante--;
        console.log(`🕒 Tiempo ajustado: ${tiempo_restante} segundos`);
    }
}


function iniciarCuentaRegresiva() {
    if (estado_actual === CONFIGURATION_MODE) {
        estado_actual = COUNTDOWN_MODE;
        start_time = Date.now(); // Guardamos el tiempo de inicio
        console.log("⏳ Cuenta regresiva iniciada");
    }
}


function actualizarTiempo() {
    if (estado_actual === COUNTDOWN_MODE) {
        let tiempo_transcurrido = Math.floor((Date.now() - start_time) / 1000);
        tiempo_restante -= tiempo_transcurrido;
        start_time = Date.now(); // Reset para la siguiente cuenta

        if (tiempo_restante <= 0) {
            tiempo_restante = 0;
            estado_actual = EXPLOSION_STATE;
            console.log("💥 ¡BOOM! La bomba explotó");
        } else {
            console.log(`⏳ Tiempo restante: ${tiempo_restante} segundos`);
        }
    }
}


function reiniciarBomba() {
    if (estado_actual === EXPLOSION_STATE) {
        estado_actual = CONFIGURATION_MODE;
        tiempo_restante = 20; // Volvemos al tiempo por defecto
        console.log("🔄 Bomba reiniciada. Modo configuración.");
    }
}


setInterval(() => {
    if (estado_actual === COUNTDOWN_MODE) {
        actualizarTiempo();
    }
}, 1000);
```

recuerda subir imagen btw
