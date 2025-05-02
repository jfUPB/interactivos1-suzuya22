# Vectores de Prueba - Bomba 3.0 (Control Dual)

## 🔍 Pruebas en `CONFIGURATION_MODE`

| Caso | Evento | Origen | Acción Esperada | Estado Final | Resultado |
|------|--------|--------|-----------------|--------------|-----------|
| 1.1 | `A` | micro:bit | +1s (max 60) | `CONFIGURATION_MODE` | ✅ |
| 1.2 | `B` | p5.js | -1s (min 10) | `CONFIGURATION_MODE` | ✅ |
| 1.3 | `A+B` | micro:bit | Iniciar cuenta | `COUNTDOWN_MODE` | ✅ |
| 1.4 | `S` | p5.js | Ignorar | `CONFIGURATION_MODE` | ✅ |
| 1.5 | `T` | micro:bit | Ignorar | `CONFIGURATION_MODE` | ✅ |

## ⏱ Pruebas en `COUNTDOWN_MODE`

| Caso | Secuencia | Origen | Resultado Esperado | Estado Final | Resultado |
|------|-----------|--------|--------------------|--------------|-----------|
| 2.1 | `A-B-A-S` | mixto | Desactivación | `CONFIGURATION_MODE` | ✅ |
| 2.2 | `A-B-S-A` | p5.js | Error | `COUNTDOWN_MODE` | ✅ |
| 2.3 | `T` | micro:bit | Ignorar | `COUNTDOWN_MODE` | ✅ |
| 2.4 | (ninguno) | - | Explosión | `EXPLOSION_STATE` | ✅ |
| 2.5 | `A-B-T-A-S` | mixto | Solo `A-B-A-S` cuenta | `CONFIGURATION_MODE` | ✅ |

## 💥 Pruebas en `EXPLOSION_STATE`

| Caso | Evento | Origen | Acción Esperada | Estado Final | Resultado |
|------|--------|--------|-----------------|--------------|-----------|
| 3.1 | `T` | p5.js | Reinicio | `CONFIGURATION_MODE` | ✅ |
| 3.2 | `A` | micro:bit | Ignorar | `EXPLOSION_STATE` | ✅ |
| 3.3 | `S` | p5.js | Ignorar | `EXPLOSION_STATE` | ✅ |

## 🔄 Pruebas de Regresión

1. **Fallo en 2.5**:
   - **Problema**: Eventos adicionales interferían con la secuencia
   - **Solución**: Limpiar buffer después de intento fallido
   ```python
   if secuencia_usuario[-4:] != secuencia_correcta:
       secuencia_usuario = []
