# Unidad 2

Parte 1: recuperación de conocimiento (Retrieval Practice)

Describe con tus palabras qué es una máquina de estados. ¿Cuáles son sus cuatro componentes fundamentales que has utilizado en esta unidad?
Explica por qué la técnica de máquina de estados es tan útil para gestionar la “concurrencia” (atender un temporizador y botones “al mismo tiempo”) en un dispositivo con un solo hilo de ejecución como el micro:bit. ¿Qué problema soluciona en comparación con usar funciones como sleep()?
Imagina que tienes que añadir una nueva funcionalidad a la bomba temporizada: si se agita (shake) el micro:bit mientras la cuenta regresiva está activa, el tiempo se reduce a la mitad. ¿Cómo modificarías tu diagrama de máquina de estados para incluir este nuevo evento y acción?
Explica qué es un “vector de prueba” y por qué es una herramienta crucial para verificar que una máquina de estados funciona como se espera.
Parte 2: reflexión sobre tu proceso (Metacognición)

¿Qué parte del diseño de la bomba temporizada te resultó más desafiante: crear el diagrama de estados (Actividad 04) o traducir ese diagrama a código MicroPython (Actividad 05)? ¿Por qué?
Describe un error o “bug” que encontraste al implementar tu programa. ¿Cómo te ayudó pensar en términos de estados, eventos y transiciones a identificar y solucionar el problema?
El problema de la bomba era complejo. ¿Qué estrategia usaste para abordarlo? ¿Comenzaste con una versión simple y añadiste funcionalidades poco a poco?
Ahora que entiendes el patrón de máquina de estados, ¿En qué otro tipo de proyecto o sistema de entretenimiento digital crees que podrías aplicarlo?
## 🤔 Fase: Reflect
Actividad 07
Coevaluación

**CORRECCIONES**

Profe aquí esta mi corrección de la actividad 04 y 05. Me pidió que corrigiera el diagrama de estados y los vectores de prueba.

**Actividad 04**  

![Imagen de WhatsApp 2025-08-15 a las 14 44 52_62f1f3a9](https://github.com/user-attachments/assets/15a11a36-8391-4f23-bca1-653f858655ad)

**Actividad 05**  

Vectores de prueba:

1. Ajuste de tiempo en estado "Configuración"

- Con el micro:bit encendido y en estado Configuración, presionar botón A repetidas veces hasta alcanzar el límite superior (60 segundos). Verificar que el valor no sobrepase 60.

- Presionar botón B repetidas veces hasta alcanzar el límite inferior (10 segundos). Verificar que no disminuya de 10.

- Alternar entre A y B para confirmar que el incremento y decremento funcionan en cualquier orden.

- Verificar que siempre se muestre en pantalla el último dígito del tiempo configurado.

2. Transición de "Configuración" a "Armado"

- Ajustar cualquier tiempo válido (10–60 segundos).

- Agitar el micro:bit (gesto “shake”) y verificar que el estado cambie a Armado, que se muestre el tiempo configurado mediante scroll y que inicie la cuenta regresiva.

3. Cuenta regresiva en "Armado"

- Confirmar que el tiempo disminuya en intervalos de 1 segundo reales.

- Verificar que el último dígito se muestre y se actualice cada 500 ms.

- Probar con valores bajos (ej. 10 segundos) y altos (ej. 60 segundos) para confirmar que la lógica funciona en ambos extremos.

4. Transición de "Armado" a "Explotado"

- Permitir que la cuenta regresiva llegue a cero.

- Verificar que la pantalla se limpie y suene la melodía WAWAWAWAA.

5. Reinicio desde "Explotado" a "Configuración"

- En estado Explotado, tocar el logo táctil.

- Confirmar que el estado vuelva a Configuración, que el tiempo se reinicie a 20 y que la pantalla quede limpia.



