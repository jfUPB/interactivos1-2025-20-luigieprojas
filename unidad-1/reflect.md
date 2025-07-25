# Unidad 1

## 🤔 Fase: Reflect

**Parte 1: recuperación de conocimiento (Retrieval Practice)**

**Basándote en los ejemplos que vimos de sistemas físicos interactivos al iniciar el curso, describe las tres características que definen a un sistema físico interactivo.**

**R/** Los sistemas físicos interactivos tienen como principal característica su estructura: inputs, outputs y un proceso. Generalmente se establecen unos márgenes de acción y aleatoriedad que la máquina (PC junto con el código) usa para generar los patrones. Estos códigos y margenes de acción son escritos por nosotros.

**Explica el modelo input-process-output de Patrick Hübner usando como ejemplo el sistema que construiste con micro:bit y p5.js en la Actividad 06. ¿Cuál fue el input, cuál fue el proceso y cuál fue el output?**

**R/** El input, que son los datos de entrada, en este caso serían la información que envía el microbit por medio del serial/cable a el ordenador, más especifícamente "N" y "A" son los datos que envía constantemente el microbit.

El proceso en este caso son los códigos de p5js y microbit editor, ya que como su nombre lo indica recoje los inputs (datos recibidos del microbit) y los procesa con su código para mostrar un output.

El output sería la pantalla del ordenador, más específicamente en p5js, ya que nos muestra el resultado de los inputs y el proceso que en este caso es el programa funcionando: un circulo que va de lado a lado si se presiona una tecla.

**¿Cuál es la función de la línea uart.write('A') en el código del micro:bit y qué función en p5.js se encarga de “escuchar” ese mensaje?**

**R/** La función de esta línea es mediante el microbit editor notificar a p5js que la recla "A" fue presionada para que pueda ser utilizada luego. P5js en el código "escucha" esta instrucción en la parte del código "let val = port.read(1);" y almacena esta información en val para luego usar ese valor para mover el círculo a la izquierda o a la derecha

**¿Cuál es la diferencia fundamental entre el arte/diseño tradicional y el arte/diseño generativo?**

**R/** Para mi hay varias diferencias muy importantes, como lo es la eficiencia del trabajo, ya que en el arte tradicional nos demorariamos mucho más tiempo del que nos tomaría en el arte generativo (eso si no contamos el tiempo en el que nos demoramos haciendo el código). Pero para mi la más fundamental es que el proceso creativo se divide entre la persona y el ordenador, mientras que en el arte tradicional el proceso es 100% de la persona. Ditgo que se divide ya que el código juega una parte muy crucial ya que el ordenador actúa con base en eso, pero también el ordenador juega una parte importante ya que se encarga del proceso y muchas decisiones que son programadas dentro de un azar.

**Imagina que quieres que un círculo en p5.js cambie a un color aleatorio cada vez que sacudes el micro:bit. Describe qué tendrías que programar en el micro:bit y qué tendrías que programar en p5.js para lograrlo.**

**R/** En el microbit supongo que tendriamos que hacer que cada cierto tiempo el microbit envie un dato a p5js de en que estado esta el microbit (si se está moviendo o no). Luego en p5js deberiamos recibir esos datos y almacenarlos en una variable, ya con eso programamos que cuando el dato que se envíe sea "x o y" por ejemplo cambie a un color aleatorio con la función random y color si mal no recuerdo.

**Parte 2: reflexión sobre tu proceso (Metacognición)**

**¿Qué fue más desafiante para ti en esta unidad: la parte conceptual (entender qué es un sistema físico interactivo) o la parte técnica (hacer que el micro:bit y p5.js se comunicaran)? ¿Por qué?**

**R/** Para mi fue más difícil la parte técnica ya que la programación se me hace complicada, más ordenar las partes del código ya que es muy importante y las funciones exactas de funciones. Aún así intenté prestar atención todo el tiempo en clase para no perderme y por ahora me ha funcionado, aunque debo admitir que todavía me cuesta.

**Describe el momento “¡Aha!” que tuviste cuando lograste que una acción en el micro:bit (presionar un botón, sacudirlo) tuviera un efecto visible en el canvas de p5.js por primera vez. ¿Qué fue lo que entendiste en ese instante?**

**R/** La verdad solo estaba siguiendo las instrucciones del profesor, así que no diría que fue 100% mérito mío, aunque cuando lo estaba haciendo al principio tuve un error, ya que no había enviado la información del microbit a microbit editor y por eso p5js no me corría adecuadamente, luego con comentario del profesor entendí esto y comprendí mucho mejor todo lo que estabamos haciendo y mi cabeza hizo un 'click'.

**Al inicio de la unidad te pregunté: “¿Este curso para qué me sirve?”. Después de experimentar y construir tu primer prototipo, ¿Cómo ha cambiado o se ha vuelto más concreta tu respuesta a esa pregunta?**

**R/** La verdad estoy muy emocionado y a la vez con ganas de superarme. Me ha parecido muy 'chevere' lo que he hecho hasta el momento, me gusta mucho el tema y siento que estoy aprendiendo cosas nuevas que me servirán en mi futuro profesional. Tambien estoy con ganas de superarme ya que siento que podría ser mucho mejor, me siento muy "noob" y eso me baja un poco el animo ya que me comparo mucho con mis otros compañeros. Sé que esto no debería ser así y que debería estar pendiente a mi propio proceso, solo que a veces es inevitable compararme. Aún así soy consciente de esto y estoy trabajando para poder mejorar.

**El tutorial de la Actividad 05 te llevó paso a paso. ¿Cómo te sentiste con ese método de aprendizaje? ¿Te dio seguridad o preferirías haberlo intentado por tu cuenta desde el principio?**

**R/**  Yo creo que debería ser una combinación de los dos, sé que por el tiempo es díficil explicar detalladamente a cada estudiante pero siento que sería mejor tener el código final aparte, e ir construyendolo en grupo (para crear ese pensamiento crítico) línea por línea entendiendolas al 100% y ya llegar al código final.
