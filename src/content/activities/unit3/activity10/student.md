## Análisis de la Implementación con Máquina de Estados
### Esto hice: 
Implementé el sistema de la bomba utilizando una máquina de estados finitos (FSM) que gestiona tres estados principales (configuración, cuenta regresiva y explosión), con transiciones bien definidas entre ellos. Esta arquitectura demostró ser particularmente poderosa por varias razones:

Gestión de Concurrencia: Al estructurar la lógica en estados discretos, cada uno procesa exclusivamente los eventos relevantes para su contexto actual. Esto elimina conflictos cuando múltiples fuentes (tanto p5.js como micro:bit) generan eventos simultáneos. Por ejemplo, durante la cuenta regresiva, la FSM ignora automáticamente los eventos de configuración, simplificando significativamente el manejo de entradas concurrentes.

Abstracción de Eventos: La máquina de estados opera independientemente del origen de los eventos. Solo procesa acciones genéricas como 'A', 'B', 'S' o 'T'. Esta abstracción permite incorporar nuevas fuentes de entrada (como Bluetooth o IoT) sin necesidad de modificar la lógica central del sistema. La escalabilidad quedó demostrada al integrar el puerto serial, donde solo fue necesario adaptar la capa de entrada/salida, sin tocar los estados principales.

Mantenibilidad y Extensibilidad: Cada estado encapsula completamente su comportamiento. Si en el futuro necesito añadir un nuevo modo (como un estado de "pausa"), puedo hacerlo definiendo sus transiciones específicas sin riesgo de afectar los otros estados existentes. Esto reduce drásticamente la probabilidad de introducir errores al escalar las funcionalidades del sistema.

Ventajas y Desventajas del Enfoque de Pruebas
Esto hice: Diseñé un conjunto exhaustivo de pruebas para cada estado, combinando eventos físicos (desde el micro:bit) y virtuales (desde p5.js), incluyendo tanto casos válidos como inválidos.

Ventajas principales:

Cobertura Integral: Las pruebas verificaron tanto el flujo ideal de operación como casos extremos y condiciones límite. Esto garantizó que la bomba respondiera consistentemente en todos los escenarios posibles.

Detección Temprana de Errores: La estrategia de probar cada estado de forma aislada permitió identificar rápidamente problemas específicos, como la interferencia de eventos adicionales durante la secuencia de desactivación.

Desventajas encontradas:

Complejidad en Pruebas de Integración: La coordinación de eventos entre p5.js y micro:bit requirió pruebas manuales adicionales que aumentaron la carga de trabajo. Una solución sería implementar scripts de automatización más sofisticados.

Dependencia del Hardware: Algunas pruebas (especialmente las que involucraban gestos como el shake) necesitaron interacción física con el micro:bit, lo que las hizo menos reproducibles y más difíciles de automatizar completamente.

Importancia Crítica de las Pruebas de Regresión
Esto hice: Después de corregir el fallo relacionado con eventos adicionales (Caso 2.5), ejecuté nuevamente toda la batería de pruebas para verificar que los cambios no hubieran introducido nuevos problemas.

Las pruebas de regresión resultaron esenciales porque:

Garantizan la Consistencia del Sistema: Cuando corregí el buffer de secuencias, no solo resolví el problema inmediato, sino que además confirmé que funcionalidades como el ajuste de tiempos o el reinicio del sistema seguían operando correctamente.

Previenen Efectos Secundarios: Estas pruebas actúan como red de seguridad, detectando cualquier impacto no deseado que los cambios puedan tener en otras partes del sistema.

Riesgos de Omitir las Pruebas de Regresión:
Si hubiera omitido este paso crucial, podrían haberse introducido errores silenciosos pero graves. Por ejemplo:

Al añadir una nueva funcionalidad como pausa ('P'), podría haber alterado inadvertidamente la lógica de la secuencia de desactivación.

Cambios en la sincronización serial podrían haber creado inconsistencias entre los estados mostrados en p5.js y el micro:bit.

Conclusión y Lecciones Aprendidas
La implementación con máquina de estados demostró ser un núcleo excepcionalmente robusto para manejar tanto la concurrencia como la diversidad de eventos. Mientras tanto, el enfoque exhaustivo de pruebas (incluyendo las pruebas de regresión) aseguró que cada iteración del desarrollo mantuviera la integridad general del sistema.

Para futuras mejoras, identifico dos áreas clave:

Automatización Avanzada: Implementar un framework de pruebas de integración más sofisticado para reducir la carga manual.

Simulación de Hardware: Desarrollar un emulador del micro:bit para hacer las pruebas más reproducibles y menos dependientes del dispositivo físico.

Esta experiencia reforzó la importancia de diseñar sistemas con arquitecturas limpias y bien definidas, junto con una estrategia de pruebas rigurosa, para crear software confiable y mantenible a largo plazo.
