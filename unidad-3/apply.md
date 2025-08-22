# Unidad 3

## 🛠 Fase: Apply

Actividad 06
Crear la bomba en p5.js
En esta actividad vas a transferir la técnica de programación con máquinas de estado a p5.js.

Crea la bomba versión 2.0 en p5.js. No olvides que al aplicar la técnica de máquinas de estado en micro:bit se evitaba colocar acciones por fuera de los eventos. En el caso de p5.js será necesario que tengas acciones por fuera de eventos porque es necesario dibujar el canvas en cada frame.

Respuesta: 

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
