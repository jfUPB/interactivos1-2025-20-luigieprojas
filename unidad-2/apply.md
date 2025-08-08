# Unidad 2

## 🛠 Fase: Apply

**Actividad 04**  

Diseño de la lógica de una bomba temporizada
Diseña la máquina de estados para solucionar el siguiente problema:

En un escape room se requiere construir una aplicación para controlar una bomba temporizada. El circuito de control de la bomba está compuesto por cuatro sensores, denominados UP (botón A), DOWN (botón B), touch (botón de touch) y ARMED (el gesto de shake de acelerómetro). Tiene dos actuadores o dispositivos de salida que serán un display (la pantalla de LEDs) y un speaker.   

**Diagrama**  

<img width="2368" height="4564" alt="image" src="https://github.com/user-attachments/assets/4ffebd85-d248-4f27-b461-816e660a9e25" />

**Actividad 05**   

Implementando la Bomba Temporizada
Implementa el código para la bomba temporizada usando mycropython y el micro:bit, incluyendo la funcionalidad básica: configuración del tiempo, cuenta regresiva y detonación.  

Reporta en un tu bitácora lo siguiente:

**1.** El código que implementa la bomba temporizada.  
**2.** La definición de los vectores de prueba básicos que permiten verificar el correcto funcionamiento del programa.

**1. R/**
```
from microbit import *
import utime
import music

class Bomba:
    def __init__(self):
        self.state = "Configuracion"
        self.tiempo = 20
        self.startTime = 0
        self.lastDisplayTime = 0

    def update(self):
        if self.state == "Configuracion":
            if button_a.was_pressed() and self.tiempo < 60:
                self.tiempo += 1
            if button_b.was_pressed() and self.tiempo > 10:
                self.tiempo -= 1
            if accelerometer.was_gesture("shake"):
                self.state = "Armado"
                self.startTime = utime.ticks_ms()
                display.scroll(str(self.tiempo))
            display.show(str(self.tiempo)[-1])

        elif self.state == "Armado":
            elapsed = utime.ticks_diff(utime.ticks_ms(), self.startTime)
            if elapsed >= 1000:
                self.tiempo -= 1
                self.startTime = utime.ticks_ms()
            if utime.ticks_diff(utime.ticks_ms(), self.lastDisplayTime) >= 500:
                display.show(str(self.tiempo)[-1])
                self.lastDisplayTime = utime.ticks_ms()
            if self.tiempo <= 0:
                self.state = "Explotado"
                display.clear()
                music.play(music.WAWAWAWAA)

        elif self.state == "Explotado":
            display.show(Image.SKULL)
            if pin_logo.is_touched():
                self.state = "Configuracion"
                self.tiempo = 20
                display.clear()

bomba = Bomba()

while True:
    bomba.update()
```
**2. R/** 

**Vector 1: Incremento normal del tiempo** 

Este caso verifica que el sistema permita configurar un tiempo de cuenta regresiva usando el botón A.

- Al encender el sistema, este inicia en el estado START y muestra un ícono de reloj.

- Luego entra al estado WAIT_BUTTONS, donde el usuario presiona dos veces el botón A para configurar el tiempo en 2.

- Al presionar A+B, inicia la cuenta regresiva (COUNTDOWN_LOOP).

- Cada segundo, el tiempo disminuye en 1 hasta llegar a cero, momento en el cual se activa la explosión.

**Vector 2: Tiempo ajustado a cero desde el inicio** 

Este caso prueba el comportamiento del sistema cuando se inicia la cuenta regresiva sin haber configurado un tiempo.

- Desde el encendido, el sistema pasa a WAIT_BUTTONS.

- El usuario presiona directamente A+B sin haber configurado tiempo (valor por defecto: 0).

- El sistema detecta tiempo = 0 y, al pasar a COUNTDOWN_CHECK, genera una explosión inmediata.

**Vector 3: Reducción del tiempo con botón B**  

Este vector comprueba la funcionalidad del botón B para reducir el tiempo antes de iniciar la cuenta regresiva.

- El usuario presiona A dos veces para configurar el tiempo en 2.

- Luego presiona B dos veces para reducir el tiempo nuevamente a 0.

- Al presionar A+B, el sistema entra en COUNTDOWN_CHECK y se activa la explosión de forma inmediata.

**Vector 4: Cuenta regresiva larga**

- Se evalúa el funcionamiento del sistema con un tiempo más prolongado.

- Después del encendido y el paso al estado WAIT_BUTTONS, el usuario presiona el botón A cinco veces para configurar el tiempo en 5.

- Al iniciar la cuenta con A+B, el sistema entra en COUNTDOWN_LOOP.

- Cada segundo el valor disminuye (5 → 4 → 3 → 2 → 1 → 0), y al llegar a cero, se genera la explosión.  
Este vector comprueba que el loop de conteo funciona correctamente en repeticiones múltiples.

**Vector 5: Uso inválido del botón B**  

Este vector verifica que el sistema maneje correctamente intentos de reducir el tiempo por debajo de cero.

- Después del encendido, el usuario presiona primero el botón B.

- Como el tiempo inicial es 0, no debe disminuir más.

- Al presionar A+B, el sistema detecta tiempo = 0 y genera la explosión inmediata.  
Este caso asegura que el tiempo mínimo permitido no sea menor a cero.
