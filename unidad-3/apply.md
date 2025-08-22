# Unidad 3

## 🛠 Fase: Apply

## Actividad 06

Crear la bomba en p5.js
En esta actividad vas a transferir la técnica de programación con máquinas de estado a p5.js.

Crea la bomba versión 2.0 en p5.js. No olvides que al aplicar la técnica de máquinas de estado en micro:bit se evitaba colocar acciones por fuera de los eventos. En el caso de p5.js será necesario que tengas acciones por fuera de eventos porque es necesario dibujar el canvas en cada frame.

**Respuesta:** 

```
let teclasValidas = "ABST";  
let bombaActiva = false;      
let estallo = false;
let desarmada = false;
let codigoIngresado = "";            
const codigoCorrecto = "ABA";  
let segundosRestantes = 0; 
let tiempoConfig = 20;  

function setup() {   
  createCanvas(400, 400);   
  textSize(16);
  segundosRestantes = tiempoConfig; 
}  

function draw() {   
  background(230);   
  textAlign(CENTER);   

  if (estallo) {
    text("¡¡BOOOOM!!", width / 2, height / 2);  
    text("Pulsa T para reiniciar", width / 2, height / 2 + 40);  
    return;
  }

  if (desarmada) {
    text("Bomba neutralizada", width / 2, height / 2);  
    text("Pulsa T para reiniciar", width / 2, height / 2 + 40);  
    return;
  }

  text("Tiempo programado: " + tiempoConfig + "s", width / 2, 40);

  if (bombaActiva) {
    let tiempo = max(0, floor(segundosRestantes - millis()/1000));
    text("¡Cuenta regresiva activa!", width / 2, height / 2 - 20);
    text("Introduce el código para desactivar", width / 2, height / 2 + 10);
    text("Tiempo: " + tiempo + "s", width / 2, height / 2 + 40);

    if (tiempo <= 0) {       
      bombaActiva = false;       
      estallo = true;     
    }   
  } else {     
    text("Presiona 'S' para armar la bomba", width / 2, height / 2 + 20);   
  } 
}

function keyPressed() {   
  let tecla = key.toUpperCase();   

  if (tecla === 'T') {
    bombaActiva = false;
    estallo = false;
    desarmada = false;
    codigoIngresado = "";
    tiempoConfig = 20;
    segundosRestantes = tiempoConfig;
    return;
  }

  if (tecla === 'A') {
    tiempoConfig += 1;
    segundosRestantes = millis()/1000 + tiempoConfig;
  }
  if (tecla === 'B') {
    if (tiempoConfig > 1) { 
      tiempoConfig -= 1;
      segundosRestantes = millis()/1000 + tiempoConfig;
    }
  }

  if (teclasValidas.includes(tecla)) {     
    if (bombaActiva) {       
      codigoIngresado += tecla;       
      if (codigoIngresado.length > 3) {         
        codigoIngresado = codigoIngresado.slice(-3);        
      }        

      if (codigoIngresado === codigoCorrecto) {         
        bombaActiva = false;         
        desarmada = true;         
        codigoIngresado = "";         
        console.log("Bomba desarmada");       
      }     
    } else {       
      if (tecla === 'S') {         
        bombaActiva = true;         
        codigoIngresado = "";         
        segundosRestantes = millis()/1000 + tiempoConfig;         
        console.log("Bomba activada");       
      }     
    }   
  } 
}
```

## Actividad 07

Bomba en p5.js + controles del micro:bit
Vas a practicar de nuevo la técnica de máquina de estados y eventos genéricos, pero esta vez vas a controlar la bomba desde el micro:bit y desde p5.js. TEN PRESENTE que la bomba estará corriendo en p5.js, pero deberás controlarla también desde los botones del micro:bit mediante el puerto serial.

**Respuesta:** 

```
let puerto;
let botonConexion;
let conexionLista = false;

let teclasPermitidas = "ABST";  
let activa = false;      
let detonada = false;
let neutralizada = false;
let entradaCodigo = "";            
const codigoClave = "ABA";  
let segundos = 0; 
let tiempoConfig = 20;  

function setup() {   
  createCanvas(400, 400);   
  textSize(16);
  segundos = tiempoConfig; 

  puerto = createSerial();
  botonConexion = createButton("Conectar micro:bit");
  botonConexion.position(100, 360);
  botonConexion.mousePressed(alternarConexion);
}  

function draw() {   
  background(230);   
  textAlign(CENTER);   
  
  if (puerto.opened() && !conexionLista) {
    puerto.clear();
    conexionLista = true;
  }

  if (detonada) {
    text("¡¡EXPLOSIÓN!!", width / 2, height / 2);  
    text("Pulsa T para reiniciar", width / 2, height / 2 + 40);  
    return;
  }

  if (neutralizada) {
    text("Bomba desactivada", width / 2, height / 2);  
    text("Pulsa T para reiniciar", width / 2, height / 2 + 40);  
    return;
  }

  text("Tiempo configurado: " + tiempoConfig + "s", width / 2, 40);

  if (activa) {
    let tiempo = max(0, floor(segundos - millis()/1000));
    text("Contador activo", width / 2, height / 2 - 20);
    text("Ingresa el código secreto", width / 2, height / 2 + 10);
    text("Tiempo restante: " + tiempo + "s", width / 2, height / 2 + 40);

    if (tiempo <= 0) {       
      activa = false;       
      detonada = true;     
    }   
  } else {     
    text("Presiona 'S' para activar bomba", width / 2, height / 2 + 20);   
  } 

  if (!puerto.opened()) {
    botonConexion.html("Conectar micro:bit");
  } else {
    botonConexion.html("Desconectar");
  }
}

function keyPressed() {   
  let tecla = key.toUpperCase();   

  // Enviar por serial solo si está conectado
  if (teclasPermitidas.includes(tecla) && puerto.opened()) {
    puerto.write(tecla);
  }

  if (tecla === 'T') {
    activa = false;
    detonada = false;
    neutralizada = false;
    entradaCodigo = "";
    tiempoConfig = 20;
    segundos = tiempoConfig;
    return;
  }

  if (tecla === 'A') {
    tiempoConfig += 1;
    segundos = millis()/1000 + tiempoConfig;
  }
  if (tecla === 'B') {
    if (tiempoConfig > 1) { 
      tiempoConfig -= 1;
      segundos = millis()/1000 + tiempoConfig;
    }
  }

  if (teclasPermitidas.includes(tecla)) {     
    console.log("Tecla presionada:" + tecla);      

    if (activa) {       
      entradaCodigo += tecla;       
      if (entradaCodigo.length > 3) {         
        entradaCodigo = entradaCodigo.slice(-3);        
      }        

      if (entradaCodigo === codigoClave) {         
        activa = false;         
        neutralizada = true;         
        entradaCodigo = "";         
        console.log("Bomba neutralizada");       
      }     
    } else {       
      if (tecla === 'S') {         
        activa = true;         
        entradaCodigo = "";         
        segundos = millis()/1000 + tiempoConfig;         
        console.log("Bomba activada");       
      }     
    }   
  } 
}

function alternarConexion() {
  if (!puerto.opened()) {
    puerto.open("MicroPython", 115200);
    conexionLista = false;
  } else {
    puerto.close();
  }
}
```
Link del editor: https://editor.p5js.org/luigieprojas/sketches/4ZsOVAVrQ 

Código micro : bit:  
```
# Imports go at the top
from microbit import *
import utime

display.clear()

class BombTask:
    def __init__(self):
        self.PASSWORD = ['A','B','A']
        self.key = ['']*len(self.PASSWORD)
        self.keyindex = 0
        self.count = 20
        self.startTime = utime.ticks_ms()
        self.state = 'CONFIG'
        display.clear()
        display.show(self.count,wait=False)

    def update(self):
        if self.state == 'CONFIG':
            if button_a.was_pressed():
                self.count = min(self.count+1,60)
                display.show(self.count,wait=False)

            if button_b.was_pressed():
                self.count = max(10,self.count-1)
                display.show(self.count, wait=False)

            if accelerometer.was_gesture('shake'):
                self.startTime = utime.ticks_ms()
                self.state = 'ARMED'

        elif self.state == 'ARMED':
            if utime.ticks_diff(utime.ticks_ms(),self.startTime) > 1000:
                self.startTime = utime.ticks_ms()
                self.count = self.count - 1
                display.show(self.count,wait=False)
                if self.count == 0:
                    display.show(Image.SKULL)
                    self.state = 'EXPLODED'

            if button_a.was_pressed():
                self.key[self.keyindex] = 'A'
                self.keyindex = self.keyindex + 1

            if button_b.was_pressed():
                self.key[self.keyindex] = 'B'
                self.keyindex = self.keyindex + 1

            if self.keyindex == len(self.key):

                passIsOK = True
                for i in range(len(self.key)):
                    if self.key[i] != self.PASSWORD[i]:
                        passIsOK = False
                        break;
                if passIsOK == True:
                    self.count = 20
                    display.show(self.count,wait=False)
                    self.keyindex = 0
                    self.state = 'CONFIG'
                else:
                    self.keyindex = 0

        elif self.state == 'EXPLODED':
            if pin_logo.is_touched():
                self.count = 20
                display.show(self.count,wait=False)
                self.startTime = utime.ticks_ms()
                self.state = 'CONFIG'

bombTask = BombTask()

while True:
    bombTask.update()
```
