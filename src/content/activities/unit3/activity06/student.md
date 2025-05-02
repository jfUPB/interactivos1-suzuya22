# Vectores de Prueba para Bomba 3.0

Basados en el diagrama de estados, probaremos todas las transiciones posibles, incluyendo casos válidos e inválidos.

## 🔹 1. Pruebas en `CONFIGURATION_MODE`

| Vector de Prueba          | Estado Inicial        | Evento                  | Acción Esperada                          | Estado Final         | ¿Pasó? |
|---------------------------|-----------------------|-------------------------|------------------------------------------|----------------------|--------|
| 1.1 Aumentar tiempo       | CONFIGURATION_MODE    | 'A' (botón A o serial)  | tiempo_restante += 1 (máx. 60)           | CONFIGURATION_MODE   | [X]    |
| 1.2 Disminuir tiempo      | CONFIGURATION_MODE    | 'B' (botón B o serial)  | tiempo_restante -= 1 (mín. 10)           | CONFIGURATION_MODE   | [X]    |
| 1.3 Iniciar cuenta regresiva | CONFIGURATION_MODE  | 'A' + 'B' (ambos botones) | Cambia a COUNTDOWN_MODE               | COUNTDOWN_MODE       | [X]    |
| 1.4 Evento inválido (shake) | CONFIGURATION_MODE   | 'S'                     | Ignorar (no acción)                     | CONFIGURATION_MODE   | [X]    |
| 1.5 Evento inválido (touch) | CONFIGURATION_MODE   | 'T'                     | Ignorar (no acción)                     | CONFIGURATION_MODE   | [X]    |

## 🔹 2. Pruebas en `COUNTDOWN_MODE`

| Vector de Prueba          | Estado Inicial        | Evento                  | Acción Esperada                          | Estado Final         | ¿Pasó? |
|---------------------------|-----------------------|-------------------------|------------------------------------------|----------------------|--------|
| 2.1 Secuencia correcta    | COUNTDOWN_MODE        | 'A', 'B', 'A', 'S'      | Desactiva bomba, vuelve a CONFIGURATION_MODE | CONFIGURATION_MODE | [X]    |
| 2.2 Secuencia incorrecta  | COUNTDOWN_MODE        | 'A', 'B', 'S', 'A'      | Muestra Image.NO, sigue cuenta regresiva | COUNTDOWN_MODE       | [X]    |
| 2.3 Evento inválido (touch) | COUNTDOWN_MODE      | 'T'                     | Ignorar (no acción)                     | COUNTDOWN_MODE       | [X]    |
| 2.4 Tiempo agotado        | COUNTDOWN_MODE        | Ninguno                 | Explota al llegar a 0                    | EXPLOSION_STATE      | [X]    |
| 2.5 Eventos adicionales   | COUNTDOWN_MODE        | 'A', 'B', 'T', 'A', 'S' | Solo importa la secuencia A → B → A → S  | CONFIGURATION_MODE (si se cumple) | [X] |

## 🔹 3. Pruebas en `EXPLOSION_STATE`

| Vector de Prueba          | Estado Inicial        | Evento                  | Acción Esperada                          | Estado Final         | ¿Pasó? |
|---------------------------|-----------------------|-------------------------|------------------------------------------|----------------------|--------|
| 3.1 Reiniciar bomba       | EXPLOSION_STATE       | 'T'                     | Vuelve a CONFIGURATION_MODE              | CONFIGURATION_MODE   | [X]    |
| 3.2 Evento inválido (botón A) | EXPLOSION_STATE    | 'A'                     | Ignorar (no acción)                     | EXPLOSION_STATE      | [X]    |
| 3.3 Evento inválido (shake) | EXPLOSION_STATE     | 'S'                     | Ignorar (no acción)                     | EXPLOSION_STATE      | [X]    |

## 🔹 4. Pruebas de Fuentes de Eventos

Verificar que tanto los sensores como el serial generan eventos correctamente:

| Vector de Prueba          | Fuente del Evento     | Evento                  | Acción Esperada                          | ¿Pasó? |
|---------------------------|-----------------------|-------------------------|------------------------------------------|--------|
| 4.1 Botón A físico        | Micro:bit             | 'A'                     | Aumenta tiempo (config) o secuencia (countdown) | [X] |
| 4.2 Botón A serial        | Puerto serial         | 'A' (mensaje)           | Mismo comportamiento que físico          | [X]    |
| 4.3 Shake físico          | Acelerómetro          | 'S'                     | Agrega a secuencia (countdown)           | [X]    |
| 4.4 Shake serial          | Puerto serial         | 'S' (mensaje)           | Mismo comportamiento que físico          | [X]    |
| 4.5 Mensaje inválido      | Puerto serial         | 'X'                     | Ignorar (no acción)                      | [X]    |

## 🔴 Fallos Detectados y Correcciones

### Fallo en 2.5:
**Problema**: Si se enviaban eventos adicionales antes de la secuencia correcta, no se reiniciaba el buffer.  
**Solución**: Limpiar `secuencia_usuario` después de un intento fallido.  
```python
if secuencia_usuario[-4:] != secuencia_correcta:
    secuencia_usuario = []  # Reiniciar para nuevo intento
