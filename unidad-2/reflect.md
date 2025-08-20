# Unidad 2

**Reflect: Consolidación y metacognición 🤔**  

**Actividad 06**   

**Autoevaluación**  

**Parte 1: recuperación de conocimiento (Retrieval Practice)**

**Describe con tus palabras qué es una máquina de estados. ¿Cuáles son sus cuatro componentes fundamentales que has utilizado en esta unidad?**  

**R/** Una máquina de estados es una forma de organizar un programa dividiéndolo en “estados” que hacen cosas diferentes, y que cambian según eventos. En esta unidad usamos: estados, eventos, acciones y transiciones.

**Explica por qué la técnica de máquina de estados es tan útil para gestionar la “concurrencia” (atender un temporizador y botones “al mismo tiempo”) en un dispositivo con un solo hilo de ejecución como el micro:bit. ¿Qué problema soluciona en comparación con usar funciones como sleep()?**  

**R/** Sirve mucho para manejar cosas “al mismo tiempo” en un micro:bit, como un temporizador y botones, porque no se queda “dormido” con sleep() y así el programa sigue respondiendo mientras cuenta el tiempo.

**Imagina que tienes que añadir una nueva funcionalidad a la bomba temporizada: si se agita (shake) el micro:bit mientras la cuenta regresiva está activa, el tiempo se reduce a la mitad. ¿Cómo modificarías tu diagrama de máquina de estados para incluir este nuevo evento y acción?**  

**R/** Si quisiera que al agitar el micro:bit en cuenta regresiva se reduzca el tiempo a la mitad, en el diagrama pondría un evento nuevo dentro del estado “Armado” que al detectar “shake” ejecute la acción de dividir el tiempo entre 2.

**Explica qué es un “vector de prueba” y por qué es una herramienta crucial para verificar que una máquina de estados funciona como se espera.**   

**R/** Un vector de prueba es una lista de pasos con entradas y salidas esperadas para ver si la máquina funciona bien. Sirve porque así comprobamos todo sin tener que adivinar si anda bien.

**Parte 2: reflexión sobre tu proceso (Metacognición)**

**¿Qué parte del diseño de la bomba temporizada te resultó más desafiante: crear el diagrama de estados (Actividad 04) o traducir ese diagrama a código MicroPython (Actividad 05)? ¿Por qué?**  

**R/** Lo más difícil fue hacer el diagrama de estados, porque hay que pensar en toda la lógica antes de programar, y la verdad hice muchas pruebas y errores creando muchos diagramas hasta que al final pude encontrar uno que se adaptara al problema.

**Describe un error o “bug” que encontraste al implementar tu programa. ¿Cómo te ayudó pensar en términos de estados, eventos y transiciones a identificar y solucionar el problema?**  

**R/** Uno de los bugs que encontré fue que, al llegar el temporizador a cero, la bomba no pasaba al estado Explotado como debía. Pensar en términos de estados, eventos y transiciones me ayudó a detectar que el evento “tiempo <= 0” estaba evaluándose en el lugar equivocado dentro del código y, por eso, no se ejecutaba la transición. Lo corregí moviendo esa verificación dentro del ciclo que gestiona el estado Armado.

**El problema de la bomba era complejo. ¿Qué estrategia usaste para abordarlo? ¿Comenzaste con una versión simple y añadiste funcionalidades poco a poco?**  

**R/** Mi estrategia fue empezar con una versión muy básica: solo dos estados y el temporizador funcionando. Una vez que esa parte estaba estable, fui agregando funciones como la configuración del tiempo, la señal visual de armado y, por último, los sonidos. Esto me permitió depurar cada bloque sin que todo se enredara.

**Ahora que entiendes el patrón de máquina de estados, ¿En qué otro tipo de proyecto o sistema de entretenimiento digital crees que podrías aplicarlo?**  

**R/** Podría usar máquinas de estados en videojuegos, por ejemplo para manejar los diferentes modos de un personaje (quieto, corriendo, atacando, saltando) sin enredarme con el código.


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

- Verificar que la pantalla se limpie y suene la melodía del micro:bit que representa la exploción.

5. Reinicio desde "Explotado" a "Configuración"

- En estado Explotado, tocar el logo táctil.

- Confirmar que el estado vuelva a Configuración, que el tiempo se reinicie a 20 y que la pantalla quede limpia.





