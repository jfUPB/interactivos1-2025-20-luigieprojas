# Unidad 1

## 🛠 Fase: Apply  

### Actividad 05  
**1. Explica cómo funciona el sistema físico interactivo que acabamos de crear.**  

**R/** Lo primero que hicimos fue crear el canva en p5js, luego decidimos crear un cuadrado. En el microbit editor primero hacemos que para confirmar que la señal está llegando el microbit se volviera una mariposa o carita feliz. Luego creamos un codigo donde cada 100 milisegundos el programa recibe "N" del serial conectado al microbit, cuando recibe esto se colorea de verde el cuadrado que habiamos creado. Por último hicimos que cuando se oprimiera el botón B del microbit este enviara "A" al programa en el browser y que este cambiara a color rojo en p5js.

### Actividad 06  

**1. Escribe el enlace a tu programa en el editor de p5.js.**  
**R/** [Enlace](https://editor.p5js.org/luigieprojas/sketches/-kIGASyyY)  
**2. Copia el código de tu programa en la bitácora (recuerda insertarlo usando markdown y el lenguaje javascript).**  
```
let port;
let connectBtn;
let connectionInitialized = false;
let x = 200;
let dir = 0;
let colorCircle = "gray";

function setup() {
  createCanvas(400, 400);
  port = createSerial();
  connectBtn = createButton("Connect to micro:bit");
  connectBtn.position(80, 300);
  connectBtn.mousePressed(connectBtnClick);
}

function draw() {
  background(220);

  if (port.opened() && !connectionInitialized) {
    port.clear();
    connectionInitialized = true;
  }

  if (port.availableBytes() > 0) {
    let val = port.read(1);

    if (val === "A") {
      dir = -1;
      colorCircle = "red";
    } else if (val === "N") {
      dir = 1;
      colorCircle = "green";
    } else if (val === "0") {
      dir = 0;
      colorCircle = "gray";
    }
  }

  x += dir * 2;

  fill(colorCircle);
  noStroke();
  ellipse(x, height / 2, 50, 50);

  if (!port.opened()) {
    connectBtn.html("Connect to micro:bit");
  } else {
    connectBtn.html("Disconnect");
  }
}

function connectBtnClick() {
  if (!port.opened()) {
    port.open("MicroPython", 115200);
    connectionInitialized = false;
  } else {
    port.close();
  }
}
```
**3. Copia el código del micro:bit en la bitácora (recuerda insertarlo usando markdown y el lenguaje python).**  
**R/** 
```
from microbit import *

uart.init(baudrate=115200)
display.show(Image.ANGRY)

while True:
    if button_a.is_pressed():
        uart.write('A')
    elif button_b.is_pressed():
        uart.write('N')
    else:
        uart.write('0')

    sleep(100)
```
