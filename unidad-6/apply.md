Reporta en tu bitácora

¿Qué ocurrió en la terminal cuando ejecutaste npm install? ¿Cuál crees que es su propósito?

<img width="579" height="364" alt="Captura de pantalla 2025-10-01 163821" src="https://github.com/user-attachments/assets/2707abe5-ea88-47e1-9c1a-b53f5a47f625" />

¿Qué mensaje específico apareció en la terminal después de ejecutar npm start? ¿Qué indica este mensaje?

<img width="578" height="366" alt="Captura de pantalla 2025-10-01 163924" src="https://github.com/user-attachments/assets/a55e808d-688f-4715-a879-461022836606" />

Describe lo que ves inicialmente en page1 y page2 en tu navegador.

<img width="1365" height="767" alt="Captura de pantalla 2025-10-01 163909" src="https://github.com/user-attachments/assets/a9a7f325-47a3-4cdb-bd75-94bc446c2f08" />

¿Qué mensajes aparecieron en la terminal del servidor cuando abriste page1 y page2?

https://github.com/user-attachments/assets/06c30a8d-41ed-48d3-8e0c-f261298eda8d 

Describe qué sucede en ambas páginas del navegador cuando mueves una de las ventanas. ¿Cambia algo visualmente? ¿Qué mensajes aparecen (si los hay) en la consola del navegador (usualmente accesible con F12 -> Pestaña Consola) y en la terminal del servidor?

Actividad 02

🧐✍️ Reporta en tu bitácora

Piensa en cómo te conectas a Internet en casa o en la Universidad. ¿Usas Wi-Fi? ¿Un cable de red? Eso es simplemente tu “rampa de acceso” a la gran red de carreteras. ¿Qué pasaría si esa rampa se corta? Anota tus ideas.

🧐✍️ Reporta en tu bitácora

¿Puedes identificar otros ejemplos de relaciones Cliente-Servidor en tu vida diaria (no necesariamente digitales)? Por ejemplo, al pedir comida en un restaurante. ¿Quién es el cliente y quién el servidor? ¿Qué se pide y qué se entrega?

🧐✍️ Reporta en tu bitácora

Toma la URL de tu sitio web favorito. Intenta identificar el protocolo, el nombre de dominio y la ruta (si la hay). ¿Qué crees que pasa si solo escribes el nombre de dominio (ej. www.google.com) sin una ruta específica? ¿Qué “página por defecto” crees que te envía el servidor0?

🧐✍️ Reporta en tu bitácora

Compara HTTP con los protocolos seriales que usaste.

¿Qué similitudes encuentras?

¿Qué diferencias clave ves?

¿Por qué crees que HTTP necesita ser más complejo que un simple envío de bytes como hacías con el micro:bit?

🧐✍️ Reporta en tu bitácora

Piensa en una página web simple, como un formulario de login.

¿Qué parte crees que es HTML (ej. los campos de texto, el botón)?
¿Qué parte es CSS (ej. el color del botón, el tipo de letra)?
¿Qué parte es JavaScript (ej. la comprobación de si escribiste algo antes de enviar, el mensaje de “contraseña incorrecta” que aparece sin recargar la página)?

🧐✍️ Reporta en tu bitácora

Compara el bucle draw() de p5.js con este modelo de “esperar a que algo pase y reaccionar”.

¿Qué ventajas crees que tiene el modelo basado en eventos para una interfaz de usuario web?
¿Sería eficiente tener un bucle draw() redibujando toda la página 60 veces por segundo si nada ha cambiado?

🧐✍️ Reporta en tu bitácora

¿Por qué crees que podría ser útil usar JavaScript tanto en el cliente (navegador) como en el servidor? ¿Se te ocurre alguna ventaja para los desarrolladores?

🧐✍️ Reporta en tu bitácora

Resume con tus propias palabras la diferencia fundamental entre una comunicación HTTP tradicional y una comunicación usando WebSockets/Socket.IO. ¿En qué tipo de aplicaciones has visto o podrías imaginar que se usa esta comunicación en tiempo real?

Actividad 03

🧐🧪✍️ Experimenta

Detén el servidor si está corriendo.

Cambia la primera ruta de /page1 a /pagina_uno.

Inicia el servidor.

Intenta acceder a http://localhost:3000/page1. ¿Funciona?

Ahora intenta acceder a http://localhost:3000/pagina_uno. ¿Funciona?

¿Qué te dice esto sobre cómo el servidor asocia URLs con respuestas? Restaura el código.

🧐🧪✍️
Experimenta

Asegúrate de que el servidor esté corriendo (npm start).

Abre http://localhost:3000/page1 en una pestaña. Observa la terminal del servidor. ¿Qué mensaje ves? Anota el ID.

Abre http://localhost:3000/page2 en OTRA pestaña. Observa la terminal. ¿Qué mensaje ves? ¿El ID es diferente?

Cierra la pestaña de page1. Observa la terminal. ¿Qué mensaje ves? ¿Coincide el ID con el que anotaste?

Cierra la pestaña de page2. Observa la terminal.

🧐🧪✍️
Experimenta

Inicia el servidor y abre page1 y page2.

Mueve la ventana de page1. Observa la terminal del servidor. ¿Qué evento se registra (win1update o win2update)? ¿Qué datos (Data:) ves?

Mueve la ventana de page2. Observa la terminal. ¿Qué evento se registra ahora? ¿Qué datos ves?

Experimento clave: cambia socket.broadcast.emit(‘getdata’, page1); por socket.emit(‘getdata’, page1); (quitando broadcast). Reinicia el servidor, abre ambas páginas. Mueve page1. ¿Se actualiza la visualización en page2? ¿Por qué sí o por qué no? (Pista: ¿A quién le envía el mensaje socket.emit?). Restaura el código a broadcast.emit.

🧐🧪✍️ Experimenta

Detén el servidor.

Cambia const port = 3000; a const port = 3001;.

Inicia el servidor. ¿Qué mensaje ves en la consola? ¿En qué puerto dice que está escuchando?

Intenta abrir http://localhost:3000/page1. ¿Funciona?

Intenta abrir http://localhost:3001/page1. ¿Funciona?

¿Qué aprendiste sobre la variable port y la función listen? Restaura el puerto a 3000.

Actividad 04

🧐🧪✍️
Experimenta

Abre page2.html en tu navegador (con el servidor corriendo).

Abre la consola de desarrollador (F12).

Detén el servidor Node.js (Ctrl+C).

Refresca la página page2.html. Observa la consola del navegador. ¿Ves algún error relacionado con la conexión? ¿Qué indica?

Vuelve a iniciar el servidor y refresca la página. ¿Desaparecen los errores?

🧐🧪✍️
Experimenta

Comenta la línea socket.emit(‘win2update’, currentPageData, socket.id); dentro del listener connect.

Reinicia el servidor y refresca page1.html y page2.html.

Mueve la ventana de page2 un poco para que envíe una actualización.

¿Qué pasó? ¿Por qué?

🧐🧪✍️
Experimenta

Abre ambas páginas (es posible que ya las tengas abiertas).

Mueve la ventana de page1. Observa la consola del navegador de page2. ¿Qué datos muestra?

Mueve la ventana de page2. Observa la consola de page1. ¿Qué pasa? ¿Por qué?

🧐🧪✍️
Experimenta

Observa checkWindowPosition() en page2.js y modifica el código del if para comprobar si el código dentreo de este se ejecuta.
Mueve cada ventana y observa las consolas.
¿Qué puedes concluir y por qué?
🧐🧪✍️
Experimenta
(¡Sé creativo!)

Cambia el background(220) para que dependa de la distancia entre las ventanas. Puedes calcular la magnitud del resultingVector usando let distancia = resultingVector.mag(); y luego usa map() para convertir esa distancia a un valor de gris o color. background(map(distancia, 0, 1000, 255, 0)); (ajusta el rango 0-1000 según sea necesario).

Inventa otra modificación creativa.
