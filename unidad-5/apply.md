### Actividad 01

- Describe cómo se están comunicando el micro:bit y el sketch de p5.js. ¿Qué datos envía el micro:bit?
R/ El micro:bit envía datos a través de la comunicación serial UART a 115200 baudios.

En cada ciclo (while True:) envía 4 valores separados por comas y terminados en salto de línea:

xValue: acelerómetro en eje X

yValue: acelerómetro en eje Y

aState: estado del botón A (True o False)

bState: estado del botón B (True o False)

- ¿Cómo es la estructura del protocolo ASCII usado?
R/ El micro:bit envía todo como texto plano ASCII (no binario).

La estructura es:

xValue,yValue,aState,bState\n


Donde:

Los valores numéricos (xValue, yValue) se envían como enteros en ASCII.

Los estados de los botones (aState, bState) se envían como "True" o "False".

Cada mensaje termina con \n (salto de línea), lo que facilita que p5.js lo lea “línea por línea”.
- Muestra y explica la parte del código de p5.js donde lee los datos del micro:bit y los transforma en coordenadas de la pantalla.
R/ 
```
if (port.availableBytes() > 0) {
  let data = port.readUntil("\n");
  if (data) {
    data = data.trim();
    let values = data.split(",");
    if (values.length == 4) {
      microBitX = int(values[0]) + windowWidth / 2;
      microBitY = int(values[1]) + windowHeight / 2;
      microBitAState = values[2].toLowerCase() === "true";
      microBitBState = values[3].toLowerCase() === "true";
      updateButtonStates(microBitAState, microBitBState);
    }
  }
}
```
port.readUntil("\n"): lee una línea completa enviada por el micro:bit.

data.split(","): separa los 4 valores que venían separados por comas.

microBitX y microBitY: convierten los valores x e y del acelerómetro en enteros y les suman la mitad de la pantalla para que la referencia quede centrada.

microBitAState y microBitBState: transforman "True" o "False" en valores booleanos.

Luego se llama a updateButtonStates(...) para generar eventos en función de los botones.
- ¿Cómo se generan los eventos A pressed y B released que se generan en p5.js a partir de los datos que envía el micro:bit?
- Capturas de pantalla de los algunos dibujos que hayas hecho con el sketch.

### Actividad 02

Abre la aplicación, configura el puerto, deja los valores por defecto y presiona Conectar. Selecciona el puerto del micro:bit (mbed Serial port) y presiona Conectar. Luego, en la sección de Recepción de Datos, en Mostrar datos como, selecciona Texto.

🧐🧪✍️ Captura el resultado del experimento anterior. ¿Por qué se ve este resultado?

Ahora cambia la opción de Mostrar datos como a Todo en Hex y vuelve a capturar el resultado.

🧐🧪✍️ Captura el resultado del experimento anterior. Lo que ves ¿Cómo está relacionado con esta línea de código?
```
data = struct.pack('>2h2B', xValue, yValue, int(aState), int(bState))
```
No te parece que el resultado es un poco más difícil de leer que el texto en ASCII?

🧐🧪✍️ ¿Qué ventajas y desventajas ves en usar un formato binario en lugar de texto en ASCII?

Ahora te voy a proponer un experimento que te permitirá ver mejor los datos. Cambia el código del micro:bit por este:
```
# Imports go at the top
from microbit import *
import struct
uart.init(115200)
display.set_pixel(0,0,9)

while True:
    if accelerometer.was_gesture('shake'):
        xValue = accelerometer.get_x()
        yValue = accelerometer.get_y()
        aState = button_a.is_pressed()
        bState = button_b.is_pressed()
        data = struct.pack('>2h2B', xValue, yValue, int(aState), int(bState))
        uart.write(data)
```
🧐🧪✍️ Captura el resultado del experimento. ¿Cuántos bytes se están enviando por mensaje? ¿Cómo se relaciona esto con el formato '>2h2B'? ¿Qué significa cada uno de los bytes que se envían?

🧐🧪✍️ Recuerda de la unidad anterior que es posible enviar números positivos y negativos para los valores de xValue y yValue. ¿Cómo se verían esos números en el formato '>2h2B'?

Ahora realiza el siguiente experimento para comparar el envío de datos en ASCII y en binario.

🧐🧪✍️ Captura el resultado del experimento. ¿Qué diferencias ves entre los datos en ASCII y en binario? ¿Qué ventajas y desventajas ves en usar un formato binario en lugar de texto en ASCII? ¿Qué ventajas y desventajas ves en usar un formato ASCII en lugar de binario?

### Actividad 03

🧐🧪✍️ Explica por qué en la unidad anterior teníamos que enviar la información delimitada y además marcada con un salto de línea y ahora no es necesario.

🧐🧪✍️ Compara el código de la unidad anterior relacionado con la recepción de los datos seriales que ves ahora. ¿Qué cambios observas?

🧐🧪✍️ Analiza el código, observa los cambios. Ejecuta y luego observa la consola. ¿Qué ves?

🧐🧪✍️ ¿Qué ves en la consola? ¿Por qué crees que se produce este error?

🧐🧪✍️ ¿Qué cambios tienen los programas y ¿Qué puedes observar en la consola del editor de p5.js?
