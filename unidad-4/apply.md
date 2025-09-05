# Apply: Aplicación 🛠

Ahora que conoces los conceptos y técnicas fundamentales para lograr una comunicación serial entre el micro:bit y un sketch en p5.js, vas a aplicarlos en un proyecto.

Actividad 5
El momento de aplicar lo aprendido
Selecciona uno de los ejemplos que exploraste en la actividad 1 y realiza las modificaciones necesarias para que interactúe con el siguiente código corriendo en el micro:bit:

```
# Imports go at the top
from microbit import *

uart.init(115200)
display.set_pixel(0,0,9)

while True:
    xValue = accelerometer.get_x()
    yValue = accelerometer.get_y()
    aState = button_a.is_pressed()
    bState = button_b.is_pressed()
    data = "{},{},{},{}\n".format(xValue, yValue, aState,bState)
    uart.write(data)
    sleep(100) # Envia datos a 10 Hz
```

Código sketch p5js:

```
sketch: "'use strict';

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
    // Detectar flanco de subida (cuando pasa de false -> true)
    if (!lastMicroBitA && microBitA) {
      // Equivale a mousePressed → nuevo layout solo una vez
      actRandomSeed = random(100000);
    }

    if (!lastMicroBitB && microBitB) {
      // Equivale a tecla "1" → cambiar color patrón izquierdo solo una vez
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
  let buffer = "";
  while (true) {
    const { value, done } = await reader.read();
    if (done) break;

    buffer += new TextDecoder().decode(value);
    let lines = buffer.split("\n");
    buffer = lines.pop();

    for (let line of lines) {
      let parts = line.trim().split(",");
      if (parts.length === 4) {
        microBitX = int(parts[0]);
        microBitY = int(parts[1]);
        microBitA = parts[2] === "True";
        microBitB = parts[3] === "True";
      }
    }
  }
}
```

Código index p5js:

```
 "<!DOCTYPE html>
<html>
  <head>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/p5.js/0.7.2/p5.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/p5.js/0.7.2/addons/p5.dom.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/p5.js/0.7.2/addons/p5.sound.min.js"></script>

    <!-- Generative Design Dependencies -->
    <script src="https://cdn.jsdelivr.net/gh/generative-design/Code-Package-p5.js@master/libraries/gg-dep-bundle/gg-dep-bundle.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/opentype.js/0.7.3/opentype.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/rita/1.3.11/rita-small.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/chroma-js/1.3.6/chroma.min.js"></script>
    <script src="https://code.jquery.com/jquery-3.3.1.min.js"></script>

    <!-- p5.webserial -->
    <script src="https://unpkg.com/p5.webserial@1.0.2/lib/p5.webserial.js"></script>

    <link rel="stylesheet" type="text/css" href="style.css" />
  </head>

  <body>
    <button onclick="connectBtnClick()">Conectar micro:bit</button>
    <script src="sketch.js"></script>
  </body>
</html>
```
Link p5js editor: https://editor.p5js.org/luigieprojas/full/BdtMbIP-S 

Link vídeo Youtube demostración: https://youtu.be/1Osor5UmV1g
