
## Evidencias de la unidad 7

# Actividad 01

<img width="1361" height="761" alt="Captura de pantalla 2025-10-10 162309" src="https://github.com/user-attachments/assets/2688bc6c-2aad-48ef-b9a6-3daae5dc4e68" /> 


<img width="1365" height="767" alt="Captura de pantalla 2025-10-10 162653" src="https://github.com/user-attachments/assets/6eb90e4a-180c-42b0-83d9-483098563a2f" />


https://github.com/user-attachments/assets/6f44b46d-3f80-4710-991c-c9834400a998

🧐🧪✍️ Reporta en tu bitácora

¿Qué URL de Dev Tunnels obtuviste? ¿Por qué crees que necesitamos usar esta URL en lugar de http://localhost:3000 o la IP local de tu computador para que el celular se conecte? 

R/ La URL que obtuve fue:
https://luisrojas-5000.use2.devtunnels.ms/
 (la tuya será distinta)

Necesitamos usar esta URL porque Dev Tunnels crea un “puente” entre Internet y nuestro servidor local (localhost).
Normalmente, http://localhost:3000 solo es accesible desde el mismo computador donde corre el servidor, no desde otro dispositivo (como el celular).

Con Dev Tunnels, VS Code expone de forma segura nuestro puerto local (3000) a Internet, de modo que el celular puede conectarse a través de una dirección HTTPS pública.
Esto es lo que permite que la página abierta en el móvil se comunique con el servidor Node.js que está corriendo localmente en el computador.

Describe brevemente qué hace npm install y npm start. 

R/ npm install: descarga e instala todas las dependencias necesarias del proyecto que están listadas en el archivo package.json (por ejemplo, express y socket.io).
Solo se ejecuta una vez, la primera vez que configuras el proyecto.

npm start: inicia el servidor Node.js ejecutando el script principal (generalmente server.js).
En este caso, arranca un servidor Express que escucha en el puerto 3000 y gestiona la comunicación entre los clientes (móvil y escritorio) mediante Socket.IO.

¿Qué mensajes observaste en la terminal del servidor al conectar el cliente de escritorio y el cliente móvil? ¿Eran diferentes los mensajes o identificadores? 

R/ 

Describe el comportamiento observado: ¿Funcionó la interacción? ¿Hubo algún retraso (latencia)?

R/ Sí, la interacción funcionó correctamente.
Al tocar y mover el dedo en la pantalla del celular, el círculo rojo en la página del computador se movía en tiempo real siguiendo los movimientos.

La respuesta fue bastante fluida, aunque hubo una ligera latencia (retraso de unos milisegundos) debido a que la información viaja por Internet (a través del túnel).

En general, la experiencia fue estable y demostró que los eventos táctiles del móvil se pueden usar como entradas interactivas remotas para aplicaciones p5.js en el computador. 

# Actividad 02 

1. ¿Por qué es necesario Dev Tunnels en este escenario y cómo funciona conceptualmente? 
R/ Dev Tunnels es necesario porque permite que un servidor local (que normalmente solo funciona dentro del mismo computador) sea accesible desde Internet.

Cuando ejecutamos el servidor con npm start, este escucha en localhost:3000, que solo puede ser alcanzado desde el propio equipo. Si tratáramos de entrar a esa dirección desde el celular, este buscaría el servidor dentro de sí mismo, no en el computador.

Dev Tunnels soluciona este problema creando un “túnel seguro” entre una URL pública y el puerto local de nuestro servidor.

De esta manera, cuando el celular accede a la URL generada por Dev Tunnels (por ejemplo, https://nombre.use2.devtunnels.ms), la conexión se redirige automáticamente hacia localhost:3000 en el computador.

Conceptualmente, funciona como si Microsoft Dev Tunnels fuera un puente intermediario: recibe las solicitudes del cliente (el celular), las envía a nuestro servidor local y luego devuelve las respuestas al cliente.

Gracias a esto, el celular puede comunicarse en tiempo real con el computador aunque estén en redes diferentes, sin exponer directamente la IP local o el puerto del servidor.

2. Describe la función de touchMoved() y por qué se usa la variable threshold en el cliente móvil.

R/ La función touchMoved() en p5.js se ejecuta automáticamente cada vez que el usuario mantiene un dedo sobre la pantalla y lo mueve.
Dentro de esta función, las variables mouseX y mouseY representan las coordenadas actuales del toque.

En este proyecto, touchMoved() captura la posición del dedo en tiempo real y envía esas coordenadas al servidor Node.js mediante Socket.IO.

Sin embargo, enviar un mensaje por cada mínimo movimiento del dedo generaría demasiados datos y podría saturar la red.

Por eso se usa una variable llamada threshold (umbral), que sirve para comprobar si el desplazamiento desde la última posición enviada es lo suficientemente grande como para justificar un nuevo envío.

Así, solo se transmiten los movimientos significativos, mejorando el rendimiento y evitando latencia innecesaria. 

3. Compara brevemente Dev Tunnels con simplemente usar la IP local. ¿Cuáles son las ventajas y desventajas de cada uno?

R/ Método: Usar IP local (ej. 192.168.1.X:3000) 

Ventajas: 
- No requiere conexión a Internet.
- La comunicación es directa y rápida en una misma red Wi-Fi.
- No depende de servicios externos.

Desventajas:
- Solo funciona si ambos dispositivos están en la misma red local.
- Puede fallar por firewalls o restricciones del router.
- No permite conexiones desde fuera de la red doméstica. 

Método: Usar Dev Tunnels

Ventajas:
- Permite acceder al servidor desde cualquier lugar con Internet.
- No requiere que el celular esté en la misma red que el computador.
- Crea un túnel seguro (HTTPS) y protege la IP real del equipo. 

Desventajas:
- Depende de una conexión estable a Internet.
- Puede presentar una ligera latencia.
- Necesita iniciar sesión en VS Code y crear el túnel cada vez que se use.

# Actividad 03

**1. - ¿Cuál es la función principal de express.static('public') en este servidor? ¿Cómo se compara con el uso de app.get('/ruta', …) del servidor de la Unidad 6?**

**R/** La función principal de express.static('public') es indicarle a Express que sirva directamente todos los archivos estáticos (HTML, CSS, JS, imágenes, etc.) que se encuentren dentro de la carpeta public.

Gracias a esto, el servidor puede entregar automáticamente las páginas del cliente móvil y del cliente de escritorio sin tener que definir rutas manualmente.

Por ejemplo, al acceder a /desktop/ o /mobile/, Express busca automáticamente esas carpetas dentro de public y entrega los archivos correspondientes.

En cambio, en la Unidad 6 se usaba app.get('/ruta', (req, res) => { ... }), donde cada ruta debía definirse manualmente con su propio código para enviar un archivo o respuesta específica.

Por tanto, express.static() simplifica el código cuando necesitamos servir un conjunto completo de archivos estáticos (como un sitio web con varias carpetas), mientras que app.get() da más control cuando queremos manejar rutas personalizadas o dinámicas. 

**2. - Explica detalladamente el flujo de un mensaje táctil: ¿Qué evento lo envía desde el móvil? ¿Qué evento lo recibe el servidor? ¿Qué hace el servidor con él? ¿Qué evento lo envía el servidor al escritorio? ¿Por qué se usa socket.broadcast.emit en lugar de io.emit o socket.emit en este caso?**

R/ Flujo completo:

Evento que lo envía desde el móvil:
En el cliente móvil (mobile/sketch.js), el evento touchMoved() se ejecuta cada vez que el usuario mueve el dedo en la pantalla.
Dentro de esta función se envía un mensaje al servidor usando 

```
socket.emit('message', { x: mouseX, y: mouseY });
```

Este mensaje contiene las coordenadas táctiles actuales.

2. Evento que lo recibe el servidor: 

En server.js, el servidor escucha el evento con:
```
socket.on('message', (message) => {
    console.log('Received message =>', message);
    socket.broadcast.emit('message', message);
});
```
Aquí el servidor recibe el objeto con las coordenadas (por ejemplo, {x: 145, y: 320}).

3. Qué hace el servidor con él: 

El servidor no lo modifica ni lo almacena; simplemente lo retransmite a los demás clientes conectados. 

4. Evento que lo envía el servidor al escritorio: 

El servidor usa
```
socket.broadcast.emit('message', message);
```
para reenviar el mensaje a todos los demás clientes conectados, excepto al que lo envió.
En este caso, el cliente móvil envía y el escritorio recibe.

5. ¿Por qué se usa socket.broadcast.emit en lugar de io.emit o socket.emit?

- socket.emit enviaría el mensaje solo al cliente que lo envió (no tiene sentido en este caso).
- io.emit lo enviaría a todos los clientes, incluyendo al que lo originó (duplicando el movimiento en el móvil).
- En cambio, socket.broadcast.emit envía el mensaje a todos los demás clientes excepto el emisor, lo cual es ideal para este flujo móvil → escritorio. 

**3. - Si conectaras dos computadores de escritorio y un móvil a este servidor, y movieras el dedo en el móvil, ¿quién recibiría el mensaje retransmitido por el servidor? ¿Por qué?**

R/ En ese caso, los dos computadores de escritorio recibirían el mensaje y el móvil no, porque el servidor usa socket.broadcast.emit().

Esto significa que el mensaje se retransmite a todos los clientes conectados excepto el que lo envió.

Como el móvil es el emisor (el que genera los datos táctiles), no necesita recibirlos. En cambio, los escritorios sí deben recibir las coordenadas para actualizar la posición del círculo en pantalla. 

**4. - ¿Qué información útil te proporcionan los mensajes console.log en el servidor durante la ejecución?** 

Los mensajes console.log permiten monitorear en tiempo real el estado de las conexiones y los datos transmitidos.

En concreto:

- "New client connected" confirma que un nuevo cliente (móvil o escritorio) se conectó correctamente al servidor.
- "Received message => {...}" muestra las coordenadas táctiles que llegan desde el móvil, permitiendo verificar que los datos se están recibiendo.
- "Client disconnected" indica que un cliente cerró su conexión o pestaña, lo que ayuda a identificar cuándo hay desconexiones.

Estos mensajes son muy útiles para depurar errores y entender el flujo de comunicación, ya que confirman que los eventos connection, message y disconnect están ocurriendo como se espera.  





