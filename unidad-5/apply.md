### Actividad 01

- **Describe cómo se están comunicando el micro:bit y el sketch de p5.js. ¿Qué datos envía el micro:bit?**
R/ El micro:bit y el sketch en p5.js se comunican a través de un canal serial (UART). El micro:bit toma continuamente tres tipos de información:

La aceleración en el eje X.

La aceleración en el eje Y.

El estado lógico de los botones A y B (true/false).

Cada ciclo de 100 ms (10 Hz), empaqueta estos datos en una cadena de texto en formato ASCII con comas como separadores y un salto de línea al final, por ejemplo:

```
-200,300,true,false\n
```
Este string viaja por el puerto serial a la computadora, donde el sketch de p5.js lo recibe y lo procesa.

**Preguntas Extra de Indagación**

¿Qué diferencias tendría este mismo sistema si usara un protocolo binario en lugar de ASCII?

¿Cómo cambiaría la eficiencia si, en vez de enviar el salto de línea, usáramos un framing basado en delimitadores especiales?

💡 Reflexión: La elección de un protocolo tan sencillo como ASCII es más costosa en bytes transmitidos, pero muy práctica para depuración y aprendizaje: cualquier usuario puede abrir un monitor serial y leer directamente los valores sin necesidad de decodificadores.

- **¿Cómo es la estructura del protocolo ASCII usado?** 

**R/** 

La estructura del protocolo se basa en delimitadores simples: 

xValue, yValue, aState, bState\n

- Cada valor está representado en texto plano (números como “-200” o palabras como “true”).
- Las comas actúan como delimitadores de campo.
- El \n (newline) indica el final de cada paquete.
Ejemplo:
123,-56,false,true\n

**Indagación extra:**

¿Qué pasaría si se omitiera el \n y el receptor no supiera dónde termina un paquete?

¿En qué casos se prefiere ASCII sobre binario aunque sea menos eficiente? (ejemplo: sistemas educativos, debugging rápido, prototipos).

💡 Reflexión: Este protocolo es muy humano y legible, pero menos robusto que alternativas binarias con checksum o delimitadores especiales. Aquí la simplicidad se priorizó sobre la eficiencia.

- **Muestra y explica la parte del código de p5.js donde lee los datos del micro:bit y los transforma en coordenadas de la pantalla.
R/**
El fragmento clave está en draw():
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

Los eventos no se envían explícitamente desde el micro:bit. Este solo transmite estados lógicos (true o false).
El sketch en p5.js compara el estado actual contra el estado previo: 
```
function updateButtonStates(newAState, newBState) {
  if (newAState === true && prevmicroBitAState === false) {
    // Evento: A pressed
    lineModuleSize = random(50, 160);
    clickPosX = microBitX;
    clickPosY = microBitY;
    print("A pressed");
  }
  if (newBState === false && prevmicroBitBState === true) {
    // Evento: B released
    c = color(random(255), random(255), random(255), random(80, 100));
    print("B released");
  }

  prevmicroBitAState = newAState;
  prevmicroBitBState = newBState;
}

```
A pressed: ocurre cuando antes estaba en false y ahora está en true.

B released: ocurre cuando antes estaba en true y ahora en false.

Esto implementa un detector de flancos, similar a cómo funcionan las interrupciones en hardware digital.

**Indagación extra:**

¿Qué pasaría si el micro:bit enviara directamente eventos (“pressed” o “released”) en lugar de estados?

¿Cuál enfoque es más flexible y por qué?

💡 Reflexión: Detectar flancos en el receptor (p5.js) permite mantener un protocolo simple en el emisor (micro:bit) y concentrar la lógica de interpretación en la aplicación.

- **Capturas de pantalla de los algunos dibujos que hayas hecho con el sketch.**

<img width="1323" height="660" alt="Captura de pantalla 2025-09-10 165829" src="https://github.com/user-attachments/assets/a26ca5c8-2d03-403f-98a7-46eb0a85fd3a" />

<img width="1321" height="660" alt="Captura de pantalla 2025-09-10 170038" src="https://github.com/user-attachments/assets/9be73e56-bec1-4eee-a4e8-e5c47cc3c649" />

<img width="1327" height="658" alt="Captura de pantalla 2025-09-10 170813" src="https://github.com/user-attachments/assets/ce5d5040-7258-4c1a-9339-fc2748e6e7eb" />


### Actividad 02

Abre la aplicación, configura el puerto, deja los valores por defecto y presiona Conectar. Selecciona el puerto del micro:bit (mbed Serial port) y presiona Conectar. Luego, en la sección de Recepción de Datos, en Mostrar datos como, selecciona Texto.

🧐🧪✍️ Captura el resultado del experimento anterior. ¿Por qué se ve este resultado?

**R/**

https://github.com/user-attachments/assets/c9b60969-bd76-43be-8d09-a37a801d8818

Cuando observé los datos en el monitor serial con la opción Texto, se muestran caracteres extraños y símbolos poco comunes como ▒☻♥ etc. Esto ocurre porque los bytes enviados ya no corresponden a caracteres ASCII legibles: ahora el micro:bit envía enteros codificados directamente en binario crudo.

Es como intentar leer un archivo de imagen (.jpg) en un bloc de notas: aparecen símbolos raros, no porque esté “dañado”, sino porque el formato ya no es texto, sino bytes binarios.

**Ahora cambia la opción de Mostrar datos como a Todo en Hex y vuelve a capturar el resultado.**

🧐🧪✍️ **Captura el resultado del experimento anterior. Lo que ves ¿Cómo está relacionado con esta línea de código?**
```
data = struct.pack('>2h2B', xValue, yValue, int(aState), int(bState))
```
**No te parece que el resultado es un poco más difícil de leer que el texto en ASCII?**

**R/**  

https://github.com/user-attachments/assets/c9de5de0-6337-4be3-bb87-6ed0d02e373b

Cuando se cambia la vista a Hexadecimal, se observa una secuencia clara de 6 bytes por paquete. Estos corresponden exactamente a la instrucción:

data = struct.pack('>2h2B', xValue, yValue, int(aState), int(bState))

2h -> dos enteros cortos (2 bytes cada uno) = 4 bytes.

2B -> dos enteros sin signo de 1 byte cada uno = 2 bytes.

Total = 6 bytes por paquete.

El orden big-endian (>) hace que el byte más significativo aparezca primero.

💡 Indagación: En ASCII, los números podían ocupar entre 2 y 6 caracteres cada uno (ej: “-1023” son 5 bytes), mientras que en binario siempre son exactamente 2 bytes. Esto hace que la transmisión sea más compacta y predecible.

🧐🧪✍️ **¿Qué ventajas y desventajas ves en usar un formato binario en lugar de texto en ASCII?**

**R/** 

Ventajas del binario:

- Mayor eficiencia: se transmiten siempre 6 bytes, sin importar si los números son grandes o pequeños.
- Menor latencia: menos datos ocupan el canal serial.
- Predecible: el receptor sabe que siempre debe leer 6 bytes.

Desventajas del binario:

- Poca legibilidad humana: no se pueden leer los datos fácilmente en un monitor serial.
- Depuración difícil: requiere herramientas para ver datos en hex o decodificarlos.

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
🧐🧪✍️ **Captura el resultado del experimento. ¿Cuántos bytes se están enviando por mensaje? ¿Cómo se relaciona esto con el formato '>2h2B'? ¿Qué significa cada uno de los bytes que se envían?**

**R/**

https://github.com/user-attachments/assets/937b6a5c-bb98-430b-95a9-0984381c0dc7

Cada mensaje tiene 6 bytes:

1. 2 bytes (short) -> xValue (valor del acelerómetro en X).
2. 2 bytes (short) -> yValue (valor del acelerómetro en Y).
3. 1 byte (unsigned) -> aState (0 = no presionado, 1 = presionado).
4. 1 byte (unsigned) -> bState.

Ejemplo: FF EC 00 3A 01 00

- FF EC = -20 en decimal -> xValue.
- 00 3A = 58 en decimal -> yValue.
- 01 = botón A presionado.
- 00 = botón B no presionado.

El formato es rígido, lo que garantiza que cada paquete sea de tamaño constante, lo cual simplifica el parsing en el receptor.


🧐🧪✍️ **Recuerda de la unidad anterior que es posible enviar números positivos y negativos para los valores de xValue y yValue. ¿Cómo se verían esos números en el formato '>2h2B'?**

**R/** 

Los enteros cortos (h) se codifican en complemento a dos.
Ejemplo:

* 25 → 00 19 en hex.
* -25 → FF E7 en hex.

Esto significa que el mismo rango positivo/negativo del acelerómetro se puede representar en binario sin pérdida de información.

💡 Indagación extra: ¿Qué ocurriría si el acelerómetro generara un valor fuera del rango de un short (–32768 a 32767)? → Se desbordaría, generando resultados corruptos. Por eso es clave elegir un tipo de dato acorde al sensor.

**Ahora realiza el siguiente experimento para comparar el envío de datos en ASCII y en binario.**

🧐🧪✍️ **Captura el resultado del experimento. ¿Qué diferencias ves entre los datos en ASCII y en binario? ¿Qué ventajas y desventajas ves en usar un formato binario en lugar de texto en ASCII? ¿Qué ventajas y desventajas ves en usar un formato ASCII en lugar de binario?**

**R/**

https://github.com/user-attachments/assets/606fcec3-7dac-4218-a8a4-4c48fdb31fb7

En el experimento donde se envían ambos formatos:

Ejemplo de salida posible:

```
Binario (Hex): FF EC 00 3A 01 00
ASCII: -20,58,True,False
```
Ventajas del binario:

- Compacto: siempre 6 bytes.
- Rápido: menor tiempo de transmisión.
- Predecible: estructura fija.

Desventajas del binario:

- Ininteligible para humanos.
- Requiere decodificadores en el receptor.

Ventajas del ASCII:

- Legible: fácil de interpretar sin herramientas.
- Flexible: admite valores de longitud variable.

Desventajas del ASCII:

- Ineficiente: “-1023” son 5 bytes frente a solo 2 en binario.
- Ambiguo: necesita delimitadores para separar campos.

ASCII es como hablar en lenguaje humano, mientras que binario es como enviar códigos Morse compactos. Ambos comunican lo mismo, pero uno es más “amigable” y el otro más “eficiente”.

### Actividad 03

🧐🧪✍️ Explica por qué en la unidad anterior teníamos que enviar la información delimitada y además marcada con un salto de línea y ahora no es necesario.

Antes se usaba salto de línea porque los paquetes tenían tamaño variable o al menos no fijo, y se necesitaba un delimitador para saber dónde acababan. Ahora los paquetes son siempre de 6 bytes (y luego de 8 con framing), así que el receptor sabe exactamente cuándo un paquete está completo → no hace falta el delimitador.

🧐🧪✍️ Compara el código de la unidad anterior relacionado con la recepción de los datos seriales que ves ahora. ¿Qué cambios observas?

Antes: port.readUntil("\n") o similar, luego hacer split() para separar campos en texto.

Ahora: port.readBytes(6) y decodificación binaria con DataView.

Es decir, pasamos de trabajar con strings delimitados a bytes de tamaño fijo.


🧐🧪✍️ ¿Qué ves en la consola? ¿Por qué crees que se produce este error?

R/ 

https://github.com/user-attachments/assets/3130a4d1-7cd4-44f9-a739-066b055159d7

En la consola se ven números correctos mezclados con valores erróneos (ej. microBitY: 513 o 3073). Esto pasa porque a veces readBytes(6) leía bytes a mitad de un paquete → se “corrían” los campos. Eso es un error de desalineación/sincronización.

🧐🧪✍️ Analiza el código, observa los cambios. Ejecuta y luego observa la consola. ¿Qué ves?

R/ 

https://github.com/user-attachments/assets/23701393-f164-46bf-b0d7-bdee5ea07557

Con framing + checksum, en consola ya ves siempre lo mismo (microBitX=500, microBitY=524...). Ya no hay corrupción. En la vista previa, los dibujos se concentran en el mismo punto, formando la figura sólida, porque los datos son constantes. El comportamiento confirma que la sincronización se resolvió.

🧐🧪✍️ ¿Qué cambios tienen los programas y ¿Qué puedes observar en la consola del editor de p5.js?

R/ 

https://github.com/user-attachments/assets/e7d1cbf0-bcf7-4a75-8bc8-a601ef0d0d5b

Con el framing y el checksum implementados, la comunicación vuelve a ser óptima:

- En consola: los valores llegan completos, alineados y sin “corrimientos raros” como antes.
- En la pantalla de p5.js: ya se pueden dibujar las figuras tal como en el caso de ASCII, pero ahora con datos binarios compactos.
- El sistema detecta si llega un paquete corrupto gracias al checksum, y simplemente lo descarta, evitando que se dibujen cosas en lugares incorrectos.

Esto significa que el protocolo ahora tiene las tres cualidades clave:

- Delimitación clara (los bytes 0x7E marcan inicio y fin).
- Tamaño fijo de payload (6 bytes de datos siempre).
- Verificación de integridad (checksum de un byte).

Por eso en este punto ya se puede dibujar normalmente como en la unidad anterior, pero con la ventaja de que los errores de transmisión no afectan la lógica ni la visualización.

### Actividad 04

Aplica lo aprendido

Vas a modificar la misma aplicación de la fase de aplicación de la unidad anterior para que soporte el protocolo de datos binarios. La aplicación del micro:bit debe ser la misma que usaste en la actividad anterior:

```
from microbit import *
import struct

uart.init(115200)
display.set_pixel(0, 0, 9)

while True:
    xValue = accelerometer.get_x()
    yValue = accelerometer.get_y()
    aState = button_a.is_pressed()
    bState = button_b.is_pressed()
    data = struct.pack('>2h2B', xValue, yValue, int(aState), int(bState))
    checksum = sum(data) % 256
    packet = b'\xAA' + data + bytes([checksum])
    uart.write(packet)
    sleep(100)
```

**Respuesta:** 


https://github.com/user-attachments/assets/5df51c08-0f9a-4d9b-a97b-4653a937876f


código skectch p5js:

```
'use strict';

// ------------------ Variables micro:bit ------------------
let port, reader;
let microBitX = 0;
let microBitY = 0;
let microBitA = false;
let microBitB = false;

// Variables para detectar flancos (cambio de estado)
let lastMicroBitA = false;
let lastMicroBitB = false;

const WAIT_MICROBIT_CONNECTION = 0;
const RUNNING = 1;
let state = WAIT_MICROBIT_CONNECTION;

// ------------------ Generative Design vars ------------------
var tileCount = 1;
var actRandomSeed = 0;

var colorLeft;
var colorRight;

var alphaLeft = 0;
var alphaRight = 100;

var transparentLeft = false;
var transparentRight = false;

// ------------------ Setup ------------------
function setup() {
  createCanvas(600, 600);
  colorMode(HSB, 360, 100, 100, 100);

  colorRight = color(0, 0, 0, alphaRight);
  colorLeft = color(323, 100, 77, alphaLeft);
}

// ------------------ Draw ------------------
function draw() {
  if (state === WAIT_MICROBIT_CONNECTION) {
    background(200);
    fill(0);
    textAlign(CENTER, CENTER);
    text("Haz click en 'Conectar micro:bit'", width / 2, height / 2);
    return;
  }

  if (state === RUNNING) {
    clear();

    // Reemplazar mouse con inclinación del micro:bit
    let mappedX = map(microBitX, -1024, 1024, 0, width);
    let mappedY = map(microBitY, -1024, 1024, 0, height);

    strokeWeight(mappedX / 15);
    randomSeed(actRandomSeed);
    tileCount = mappedY / 15;

    for (var gridY = 0; gridY < tileCount; gridY++) {
      for (var gridX = 0; gridX < tileCount; gridX++) {
        var posX = width / tileCount * gridX;
        var posY = height / tileCount * gridY;

        alphaLeft = transparentLeft ? gridY * 10 : 100;
        colorLeft = color(
          hue(colorLeft),
          saturation(colorLeft),
          brightness(colorLeft),
          alphaLeft
        );

        alphaRight = transparentRight ? 100 - gridY * 10 : 100;
        colorRight = color(
          hue(colorRight),
          saturation(colorRight),
          brightness(colorRight),
          alphaRight
        );

        var toggle = int(random(0, 2));

        if (toggle == 0) {
          stroke(colorLeft);
          line(
            posX,
            posY,
            posX + (width / tileCount) / 2,
            posY + height / tileCount
          );
          line(
            posX + (width / tileCount) / 2,
            posY,
            posX + width / tileCount,
            posY + height / tileCount
          );
        }

        if (toggle == 1) {
          stroke(colorRight);
          line(
            posX,
            posY + width / tileCount,
            posX + (height / tileCount) / 2,
            posY
          );
          line(
            posX + (height / tileCount) / 2,
            posY + width / tileCount,
            posX + (height / tileCount),
            posY
          );
        }
      }
    }

    // --------- Micro:bit buttons control (solo en "click") ---------
    if (!lastMicroBitA && microBitA) {
      actRandomSeed = random(100000);
    }

    if (!lastMicroBitB && microBitB) {
      if (colorsEqual(colorLeft, color(273, 73, 51, alphaLeft))) {
        colorLeft = color(323, 100, 77, alphaLeft);
      } else {
        colorLeft = color(273, 73, 51, alphaLeft);
      }
    }

    // Actualizar estados anteriores
    lastMicroBitA = microBitA;
    lastMicroBitB = microBitB;
  }
}

// ------------------ Mouse & Keys originales ------------------
function mousePressed() {
  actRandomSeed = random(100000);
}

function keyReleased() {
  if (key == 's' || key == 'S') saveCanvas(gd.timestamp(), 'png');

  if (key == '1') {
    if (colorsEqual(colorLeft, color(273, 73, 51, alphaLeft))) {
      colorLeft = color(323, 100, 77, alphaLeft);
    } else {
      colorLeft = color(273, 73, 51, alphaLeft);
    }
  }

  if (key == '2') {
    if (colorsEqual(colorRight, color(0, 0, 0, alphaRight))) {
      colorRight = color(192, 100, 64, alphaRight);
    } else {
      colorRight = color(0, 0, 0, alphaRight);
    }
  }

  if (key == '3') transparentLeft = !transparentLeft;
  if (key == '4') transparentRight = !transparentRight;

  if (key == '0') {
    transparentLeft = false;
    transparentRight = false;
    colorLeft = color(323, 100, 77, alphaLeft);
    colorRight = color(0, 0, 0, alphaRight);
  }
}

function colorsEqual(col1, col2) {
  return col1.toString() == col2.toString();
}

// ------------------ Serial Communication ------------------
async function connectBtnClick() {
  try {
    port = await navigator.serial.requestPort();
    await port.open({ baudRate: 115200 });
    reader = port.readable.getReader();
    state = RUNNING;
    readLoop();
  } catch (err) {
    console.error("Error al conectar:", err);
  }
}

async function readLoop() {
  let buffer = new Uint8Array();

  while (true) {
    const { value, done } = await reader.read();
    if (done) break;
    if (!value) continue;

    // Concatenar al buffer existente
    let tmp = new Uint8Array(buffer.length + value.length);
    tmp.set(buffer, 0);
    tmp.set(value, buffer.length);
    buffer = tmp;

    // Procesar mientras haya 8 bytes disponibles
    while (buffer.length >= 8) {
      // Buscar cabecera 0xAA
      let start = buffer.indexOf(0xAA);
      if (start === -1) {
        buffer = new Uint8Array();
        break;
      }

      if (buffer.length < start + 8) break;

      // Extraer paquete
      let packet = buffer.slice(start, start + 8);
      buffer = buffer.slice(start + 8);

      // Validar checksum
      let data = packet.slice(1, 7);
      let checksum = packet[7];
      let sum = 0;
      for (let b of data) sum += b;
      if (sum % 256 !== checksum) {
        console.warn("Checksum incorrecto, paquete descartado");
        continue;
      }

      // Decodificar datos
      let dv = new DataView(packet.buffer, packet.byteOffset, packet.byteLength);
      microBitX = dv.getInt16(1, false); // big-endian
      microBitY = dv.getInt16(3, false);
      microBitA = dv.getUint8(5) === 1;
      microBitB = dv.getUint8(6) === 1;
    }
  }
}
```
En esta actividad partimos de la misma aplicación de la fase de aplicación de la unidad 4, que funcionaba con datos enviados como texto. Allí el micro:bit mandaba valores del acelerómetro y botones en formato delimitado por saltos de línea, y p5.js podía leerlos directamente como strings con readLine().

El reto de la unidad 5 fue modificar esa misma aplicación para que ahora funcionara con un protocolo de datos binarios. Esto implicó dos cambios principales:

1. En el micro:bit (emisor):

- Usamos el módulo struct para empaquetar los datos en binario.
- Definimos un formato fijo >2h2B, que incluye:

  - xValue y yValue (acelerómetro, 16 bits cada uno).
  - aState y bState (botones A y B, 8 bits cada uno).

- Añadimos un checksum (suma de bytes % 256) para garantizar la integridad del paquete.
- Cada paquete empieza con una cabecera 0xAA, que permite al receptor sincronizar la lectura.

Ejemplo de envío en micro : bit:
```
data = struct.pack('>2h2B', xValue, yValue, int(aState), int(bState))
checksum = sum(data) % 256
packet = b'\xAA' + data + bytes([checksum])
uart.write(packet)
```
En p5.js (receptor):

- En lugar de leer texto línea a línea, ahora usamos DataView para interpretar directamente los bytes recibidos.
- Implementamos un bucle de lectura (readLoop) que detecta la cabecera 0xAA, extrae los siguientes 6 bytes de datos y el byte de checksum.
- Verificamos la integridad comparando la suma de los datos con el checksum recibido.
- Convertimos los bytes en enteros (getInt16, getUint8) para recuperar los valores originales.

Fragmento clave en p5.js: 

```
if (value.getUint8(0) === 0xAA) {
  let xValue = value.getInt16(1);
  let yValue = value.getInt16(3);
  let aState = value.getUint8(5);
  let bState = value.getUint8(6);
  let checksum = value.getUint8(7);
}
```
Relación con lo visto en la unidad 5:

- Protocolos binarios: reemplazamos texto delimitado por paquetes compactos de bytes.
- Cabecera y checksum: conceptos claves de esta unidad para estructurar la comunicación y validar errores.
- Empaquetado/Desempaquetado: uso de struct en Python y DataView en JS para leer valores binarios.
- Máquina de estados implícita: el receptor pasa por estados buscando cabecera -> leyendo datos -> verificando checksum.

Acá esta un códgio beta/intermedio que logré rescatar y obviamente tiene varios errores que les puse un comentario para verlos más rápidamente:

```
let connectBtn;
let isConnected = false;

function setup() {
  createCanvas(400, 200);
  background(220);

  // Crear botón
  connectBtn = createButton("Conectar Micro:bit");
  connectBtn.position(20, 20);

  // ❌ ERROR: aquí llamamos a una función que no existe aún
  connectBtn.mousePressed(connectBtnClick);
}

function draw() {
  background(220);
  textSize(16);

  // Mostrar estado en pantalla
  if (isConnected) {
    text("Micro:bit conectado ✅", 20, 100);
  } else {
    text("Esperando conexión...", 20, 100);
  }
}

// ❌ ERROR: Olvidamos definir connectBtnClick()
// No existe ninguna función que maneje la conexión
```

Error 1:

ReferenceError: connectBtnClick is not defined
Causa: Llamamos a connectBtnClick en mousePressed sin haber creado la función.
Solución: Crear la función connectBtnClick() que cambie el estado de isConnected.

Error 2:

El estado isConnected nunca cambia.
Causa: Aunque la variable existe, nunca la modificamos.
Solución: En connectBtnClick(), asignar isConnected = true;.

Error 3:

La aplicación no muestra intentos fallidos de conexión (poca retroalimentación).
Causa: No hay un contador ni mensajes dinámicos.
Solución: Añadir una variable intentos y mostrar cuántas veces se presionó el botón.

