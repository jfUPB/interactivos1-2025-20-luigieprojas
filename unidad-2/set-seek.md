# Unidad 2

## 🔎 Fase: Set + Seek

Actividad 01
Analizando un programa con una máquina de estados simple
Analicemos juntos el siguiente código identificando estados, eventos y acciones. Responde las preguntas planteadas.

```
from microbit import *
import utime

class Pixel:
    def __init__(self,pixelX,pixelY,initState,interval):
        self.state = "Init"
        self.startTime = 0
        self.interval = interval
        self.pixelX = pixelX
        self.pixelY = pixelY
        self.pixelState = initState

    def update(self):

        if self.state == "Init":
            self.startTime = utime.ticks_ms()
            self.state = "WaitTimeout"
            display.set_pixel(self.pixelX,self.pixelY,self.pixelState)

        elif self.state == "WaitTimeout":
            if utime.ticks_diff(utime.ticks_ms(),self.startTime) > self.interval:
                self.startTime = utime.ticks_ms()
                if self.pixelState == 9:
                    self.pixelState = 0
                else:
                    self.pixelState = 9
                display.set_pixel(self.pixelX,self.pixelY,self.pixelState)

pixel1 = Pixel(0,0,0,1000)
pixel2 = Pixel(4,4,0,500)

while True:
    pixel1.update()
    pixel2.update()
```
- **Describe detalladamente cómo funciona este ejemplo:**
  
**R/** Este programa está hecho para Micro:bit y hace que dos píxeles del display se enciendan y apaguen en diferentes intervalos de tiempo usando una máquina de estados simple.

Se define una clase llamada Pixel que representa un píxel específico en la pantalla LED de 5x5. A cada objeto Pixel se le asigna una posición (pixelX, pixelY), un estado inicial del brillo (initState, que puede ser 0 o 9), y un tiempo de espera (interval) que define cada cuánto tiempo cambia su estado.

Cada Pixel empieza en el estado "Init", y en cuanto se llama al método update() por primera vez, cambia al estado "WaitTimeout" y se muestra el píxel con el brillo correspondiente. Desde ese momento, cada vez que se llama a update(), si ha pasado el tiempo definido en interval, se cambia el brillo del píxel: si estaba encendido (9), se apaga (0), y viceversa.

El ciclo principal while True: llama constantemente al método update() de ambos píxeles para que su comportamiento se actualice de forma independiente. Por ejemplo, pixel1 parpadea cada 1000 ms (1 segundo) y pixel2 cada 500 ms.

- **¿Cuáles son los estados en el programa?**
  
**R/** Los estados definidos son:

Init: Estado inicial. Se configura el tiempo de inicio y se enciende el píxel con el valor inicial.

WaitTimeout: Estado donde espera a que se cumpla el intervalo para alternar el valor del píxel.

- **¿Cuáles son los eventos/inputs en el programa?**
  
**R/** Aunque no hay entradas del usuario como botones o sensores, sí hay eventos temporales basados en el tiempo:

Inicio del programa: cuando se llama por primera vez a update(), lo que provoca la transición de "Init" a "WaitTimeout".

Evento de timeout: cuando ha pasado más tiempo que el valor de interval, lo que provoca el cambio del brillo del píxel.

- **¿Cuáles son las acciones en el programa?**
  
**R/** Las acciones que ocurren en función de los estados y eventos son:

En el estado "Init":

Se guarda el tiempo actual (startTime).

Se cambia al estado "WaitTimeout".

Se muestra el píxel con el valor inicial (display.set_pixel(...)).

En el estado "WaitTimeout":

Si ha pasado el tiempo (interval), se reinicia el tiempo de espera.

Se alterna el valor del píxel (de 0 a 9 o de 9 a 0).

Se actualiza el display con el nuevo valor del píxel.


