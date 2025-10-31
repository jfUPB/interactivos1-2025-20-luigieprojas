
# Evidencias de la unidad 8

**Inspiración:**

Mi reto se inspiro originalmente en guitar hero, aunque no se puede apreciar bien el porque a simple vista, con los botones del celular intenta hacer que se cambiara la animación del computador, justamente como se hace en guitar hero cuando logras un combo.

**Estructura archivos Reto:**

<img width="220" height="730" alt="image" src="https://github.com/user-attachments/assets/13f8ad33-12fe-45d6-b486-5c0c35822caf" />

**Códigos Archivos:**

**desktop/index.html/:**

```
<!DOCTYPE html>
<html lang="es">
<head>
 <meta charset="UTF-8">
 <title>Bad Guy Interactive - Desktop</title>

 <script src="/socket.io/socket.io.js"></script>

 <script src="https://cdnjs.cloudflare.com/ajax/libs/p5.js/1.6.0/p5.min.js"></script>
 <script src="https://cdnjs.cloudflare.com/ajax/libs/p5.js/1.6.0/addons/p5.sound.min.js"></script>

 <script src="sketch.js"></script>
</head>
<body>
</body>
</html>
``` 

**public/desktop/sketch.js/:** 

```
// public/desktop/sketch.js

let socket;

// Frames y audio
let pianoFrames = [];
let billieFrames = [];
let badGuy;

// Animación
let frameIndex = 0;
// 167ms por frame (~6 FPS)
const frameDuration = 167;
let animationLoop;

// Estado
let currentPerspective = 'billie'; // 'billie' o 'piano'
let mood = false; // true = combo (frames 1-5), false = normal (frames 1-3)
let comboMode = 'none'; // 'none' | 'A' | 'B'

// ----------------------------------------------------
// 🔹 PRELOAD: cargar imágenes y audio
// ----------------------------------------------------
function preload() {
    try {
        for (let i = 1; i <= 9; i++) {
            billieFrames.push(loadImage(`images/billie${i}.jpg`));
            pianoFrames.push(loadImage(`images/piano${i}.jpg`));
        }
        badGuy = loadSound(
            'images/badGuy.mp3',
            () => console.log('Audio cargado'),
            (err) => console.error('Fallo al cargar audio:', err)
        );
    } catch (error) {
        console.error("Error al cargar assets. Revisa las rutas y nombres.", error);
    }
}

// ----------------------------------------------------
// 🔹 SETUP: crear canvas y configurar socket
// ----------------------------------------------------
function setup() {
    createCanvas(windowWidth, windowHeight);
    socket = io();

    animationLoop = setInterval(animateCharacter, frameDuration);

    // --- Eventos desde el móvil ---
    socket.on('switchPerspective', () => {
        currentPerspective = currentPerspective === 'piano' ? 'billie' : 'piano';
        frameIndex = 0;
        comboMode = 'none';
    });

    socket.on('toggleCombo', (isComboActive) => {
        mood = isComboActive;
        comboMode = 'none'; // reset al desactivar combo
        frameIndex = 0;
    });

    // --- Eventos desde el micro:bit ---
    socket.on('comboA', () => {
        if (!mood) return; // ignorar si combo no está activo
        if (comboMode === 'A') {
            comboMode = 'none';
            console.log("Desactivando Combo A (vuelve a combo simple)");
        } else {
            comboMode = 'A';
            console.log("Combo A activo");
        }
        frameIndex = 0;
    });

    socket.on('comboB', () => {
        if (!mood) return;
        if (comboMode === 'B') {
            comboMode = 'none';
            console.log("Desactivando Combo B (vuelve a combo simple)");
        } else {
            comboMode = 'B';
            console.log("Combo B activo");
        }
        frameIndex = 0;
    });

    console.log("Desktop setup completo. Haz clic para iniciar la música.");
}

// ----------------------------------------------------
// 🔹 DRAW: dibujar frame actual
// ----------------------------------------------------
function draw() {
    background(20);

    let currentSet = (currentPerspective === 'billie') ? billieFrames : pianoFrames;
    let frameToDraw = currentSet[frameIndex];

    if (frameToDraw) {
        // Mantener proporción
        let aspect = frameToDraw.width / frameToDraw.height;
        let newW = width;
        let newH = width / aspect;
        if (newH > height) {
            newH = height;
            newW = height * aspect;
        }
        image(frameToDraw, (width - newW) / 2, (height - newH) / 2, newW, newH);
    } else {
        textSize(40);
        fill(255, 0, 0);
        textAlign(CENTER, CENTER);
        text("ERROR: Frame no encontrado. ¿Rutas correctas?", width / 2, height / 2);
    }

    // Indicadores visuales
    drawIndicators();
}

// ----------------------------------------------------
// 🔹 FUNCIONES AUXILIARES
// ----------------------------------------------------
function animateCharacter() {
    let maxFrames = 3; // base

    if (mood) {
        // Combo activado
        if (comboMode === 'A') maxFrames = 7;     // 1–7
        else if (comboMode === 'B') maxFrames = 9; // 1–9
        else maxFrames = 5;                       // combo simple 1–5
    }

    frameIndex = (frameIndex + 1) % maxFrames;
}

function drawIndicators() {
    // Indicador de Combo (desde móvil)
    if (mood) {
        textSize(80);
        fill(255, 255, 0);
        textAlign(LEFT, TOP);
        text('C', 50, 50);
    }

    // Indicador del modo extendido (A o B)
    if (comboMode === 'A') {
        textSize(80);
        fill(0, 255, 0);
        textAlign(LEFT, TOP);
        text('A', 130, 50);
    } else if (comboMode === 'B') {
        textSize(80);
        fill(0, 150, 255);
        textAlign(LEFT, TOP);
        text('B', 130, 50);
    }

    // Perspectiva
    textSize(30);
    fill(255);
    textAlign(LEFT, TOP);
    text(`Perspective: ${currentPerspective.toUpperCase()}`, 50, height - 50);

    // Indicador para iniciar música
    if (badGuy && badGuy.isLoaded() && !badGuy.isPlaying()) {
        textSize(40);
        fill(0, 255, 255);
        textAlign(CENTER, CENTER);
        text("HAZ CLIC PARA INICIAR MÚSICA", width / 2, height / 2 + 100);
    }
}

function mousePressed() {
    if (badGuy && badGuy.isLoaded() && !badGuy.isPlaying()) {
        badGuy.loop();
    }
}

function windowResized() {
    resizeCanvas(windowWidth, windowHeight);
}
// --- SIMULADOR DE MICROBIT ---
window.addEventListener('keydown', (e) => {
  if (e.key === 'a') {
    console.log('Simulando botón A');
    socket.emit('comboA');
  }
  if (e.key === 'b') {
    console.log('Simulando botón B');
    socket.emit('comboB');
  }
});
``` 

**public/mobile/index.html/:**

```
<!DOCTYPE html>
<html lang="es">
<head>
 <meta charset="UTF-8">
 <title>Bad Guy Interactive - Mobile</title>
 <script src="/socket.io/socket.io.js"></script>
 </head>
<body>
    <h1>Controlador</h1>
    <button onclick="switchPerspective()">Cambiar Perspectiva</button>
    <button id="comboBtn" onclick="toggleCombo(this)">Activar Combo</button>
    <script src="sketch.js"></script>
</body>
</html>
```
**public/mobile/sketch.js/:**

```
// public/mobile/sketch.js

// Se conecta automáticamente al servidor que sirve este HTML (localhost:3000)
const socket = io(); 

let isComboActive = false;
let currentPerspective = 'billie'; // Estado local para alternar el texto del botón

function switchPerspective() {
    // Alternar el estado local de la perspectiva para actualizar el texto del botón si quieres
    currentPerspective = currentPerspective === 'billie' ? 'piano' : 'billie';

    // Opcional: Actualizar el texto del botón aquí si lo deseas
    // const btn = document.querySelector('button:first-of-type');
    // btn.textContent = `Cambiar a ${currentPerspective === 'billie' ? 'Pianista' : 'Billie'}`;

    // Envía el evento al servidor para que el escritorio cambie la animación
    socket.emit('switchPerspective');
    console.log('Perspectiva cambiada a:', currentPerspective);
}

function toggleCombo(buttonElement) {
    isComboActive = !isComboActive;
    
    // Actualiza el estilo y texto del botón
    if (isComboActive) {
        buttonElement.textContent = 'Desactivar Combo';
        buttonElement.style.backgroundColor = 'green';
        buttonElement.style.color = 'white';
    } else {
        buttonElement.textContent = 'Activar Combo';
        buttonElement.style.backgroundColor = ''; // Restablecer estilo
        buttonElement.style.color = '';
    }

    // Envía el estado del combo (true/false) al servidor
    socket.emit('toggleCombo', isComboActive);
    console.log('Combo toggled:', isComboActive ? 'ON' : 'OFF');
}
```
**server.js:** 

```
// ============================================================
// 🟢 SERVER.JS — Bad Guy Interactive (seguro con o sin micro:bit)
// ============================================================

const express = require('express');
const http = require('http');
const socketIO = require('socket.io');
const path = require('path');

const app = express();
const server = http.createServer(app);
const io = socketIO(server);
const port = 3000;

// ----------------------------------------------------
// SERVIR ARCHIVOS ESTÁTICOS
// ----------------------------------------------------
app.use(express.static('public'));

// Rutas directas para escritorio y móvil
app.get('/desktop', (req, res) => {
  res.sendFile(path.join(__dirname, 'public', 'desktop', 'index.html'));
});

app.get('/mobile', (req, res) => {
  res.sendFile(path.join(__dirname, 'public', 'mobile', 'index.html'));
});

// ----------------------------------------------------
// SOCKET.IO — COMUNICACIÓN ENTRE CLIENTES
// ----------------------------------------------------
io.on('connection', (socket) => {
  console.log('🟢 Nuevo cliente conectado');

  // --- Desde el móvil ---
  socket.on('switchPerspective', () => {
    console.log('📲 switchPerspective');
    socket.broadcast.emit('switchPerspective');
  });

  socket.on('toggleCombo', (isCombo) => {
    console.log(`📲 toggleCombo (${isCombo})`);
    socket.broadcast.emit('toggleCombo', isCombo);
  });

  // --- Simulador / Microbit ---
  socket.on('comboA', () => {
    console.log('🧩 comboA recibido → reenviando a todos');
    io.emit('comboA');
  });

  socket.on('comboB', () => {
    console.log('🧩 comboB recibido → reenviando a todos');
    io.emit('comboB');
  });

  socket.on('disconnect', () => {
    console.log('🔴 Cliente desconectado');
  });
});

// ----------------------------------------------------
// INTENTO DE DETECTAR MICRO:BIT (sin romper el server)
// ----------------------------------------------------
(async () => {
  try {
    const { SerialPort } = await import('serialport');
    const { ReadlineParser } = await import('@serialport/parser-readline');

    const ports = await SerialPort.list();
    const microbitPortInfo = ports.find(p =>
      p.manufacturer && p.manufacturer.toLowerCase().includes('mbed')
    );

    if (microbitPortInfo) {
      console.log(`💡 Micro:bit detectado en ${microbitPortInfo.path}`);
      const microbit = new SerialPort({ path: microbitPortInfo.path, baudRate: 115200 });
      const parser = microbit.pipe(new ReadlineParser({ delimiter: '\r\n' }));

      parser.on('data', (data) => {
        const msg = data.trim();
        console.log(`📡 Microbit dice: ${msg}`);
        if (msg === 'A') io.emit('comboA');
        else if (msg === 'B') io.emit('comboB');
      });

      microbit.on('error', (err) => {
        console.error('⚠️ Error del micro:bit:', err.message);
      });
    } else {
      console.log('⚠️ No se detectó micro:bit. Modo simulador activo.');
    }
  } catch (err) {
    console.log('⚠️ No se pudo inicializar soporte del micro:bit. (quizás no está instalado serialport)');
  }
})();

// ----------------------------------------------------
// INICIAR SERVIDOR
// ----------------------------------------------------
server.listen(port, () => {
  console.log(`🚀 Servidor funcionando en: http://localhost:${port}`);
  console.log('🧠 Si no se detecta micro:bit, usa teclas A y B para simular.');
});
```
**código micro bit python editor:**

```
from microbit import *

# Muestra una carita feliz cuando inicia
display.show(Image.HAPPY)

while True:
    if button_a.was_pressed():
        display.show("A")
        # Envía un mensaje al puerto serial (lo leerá el server.js)
        uart.write("A\n")
        sleep(300)
        display.show(Image.HAPPY)

    if button_b.was_pressed():
        display.show("B")
        uart.write("B\n")
        sleep(300)
        display.show(Image.HAPPY)
```

**Evidencia Reto:**

https://github.com/user-attachments/assets/4d8eca35-e736-4e7d-84d3-c176fbc2f647




