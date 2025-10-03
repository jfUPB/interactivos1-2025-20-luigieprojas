**Actividad 01**

**🧐✍️ Reporta en tu bitácora**

**¿Qué ocurrió en la terminal cuando ejecutaste npm install? ¿Cuál crees que es su propósito?**

<img width="579" height="364" alt="Captura de pantalla 2025-10-01 163821" src="https://github.com/user-attachments/assets/2707abe5-ea88-47e1-9c1a-b53f5a47f625" />

**¿Qué mensaje específico apareció en la terminal después de ejecutar npm start? ¿Qué indica este mensaje?**

<img width="578" height="366" alt="Captura de pantalla 2025-10-01 163924" src="https://github.com/user-attachments/assets/a55e808d-688f-4715-a879-461022836606" />
```
Servidor corriendo en http://localhost:3000
```
Esto indica que el servidor Node.js se está ejecutando correctamente y está escuchando conexiones en el puerto 3000 de la máquina local. En otras palabras, ya se puede acceder a las páginas web que sirve.

Describe lo que ves inicialmente en page1 y page2 en tu navegador.

<img width="1365" height="767" alt="Captura de pantalla 2025-10-01 163909" src="https://github.com/user-attachments/assets/a9a7f325-47a3-4cdb-bd75-94bc446c2f08" />

**¿Qué mensajes aparecieron en la terminal del servidor cuando abriste page1 y page2?**

https://github.com/user-attachments/assets/06c30a8d-41ed-48d3-8e0c-f261298eda8d 

**Describe qué sucede en ambas páginas del navegador cuando mueves una de las ventanas. ¿Cambia algo visualmente? ¿Qué mensajes aparecen (si los hay) en la consola del navegador (usualmente accesible con F12 -> Pestaña Consola) y en la terminal del servidor?**

**Actividad 02**

**🧐✍️ Reporta en tu bitácora**

**Piensa en cómo te conectas a Internet en casa o en la Universidad. ¿Usas Wi-Fi? ¿Un cable de red? Eso es simplemente tu “rampa de acceso” a la gran red de carreteras. ¿Qué pasaría si esa rampa se corta? Anota tus ideas.**

**R/** En mi casa y en la universidad me conecto principalmente a Internet a través de Wi-Fi, aunque en algunos laboratorios también hay conexiones por cable de red (Ethernet). Esta conexión funciona como la “rampa de acceso” a la gran red de carreteras que es Internet. 

Si esta rampa se corta (por ejemplo: se va el Wi-Fi, se desconecta el cable, o falla el router), mi computador pierde la posibilidad de “salir a la red”, así que no podría acceder a páginas web, servicios en la nube ni comunicarme con otros servidores. Es como si el vehículo quedara atrapado en el garaje sin salida a la autopista.

**🧐✍️ Reporta en tu bitácora**

**¿Puedes identificar otros ejemplos de relaciones Cliente-Servidor en tu vida diaria (no necesariamente digitales)? Por ejemplo, al pedir comida en un restaurante. ¿Quién es el cliente y quién el servidor? ¿Qué se pide y qué se entrega?**

**R/** Un ejemplo claro es en un restaurante:

- Cliente: yo, la persona que hace el pedido.
- Servidor: el mesero o el restaurante mismo.
- Petición: ordenar un plato específico del menú.
- Respuesta: el mesero entrega el plato preparado en la mesa. 

Otro ejemplo sería en una biblioteca física:

- Cliente: la persona que busca un libro.
- Servidor: el bibliotecario o el sistema de archivos de la biblioteca.
- Petición: solicitar un libro específico.
- Respuesta: el servidor localiza y entrega el libro para consulta.

En ambos casos se sigue el mismo patrón cliente pide → servidor responde.

**🧐✍️ Reporta en tu bitácora**

Toma la URL de tu sitio web favorito. Intenta identificar el protocolo, el nombre de dominio y la ruta (si la hay). ¿Qué crees que pasa si solo escribes el nombre de dominio (ej. www.google.com) sin una ruta específica? ¿Qué “página por defecto” crees que te envía el servidor0?

R/ Mi sitio web favorito es YouTube, y su URL principal es:

```
https://www.youtube.com/
```

- Protocolo: https:// → Es el lenguaje de comunicación (HTTP seguro) que el navegador y el servidor usan para hablar entre sí.
- Nombre de dominio: www.youtube.com → Es la dirección “nombrada” del servidor, que internamente se traduce a una IP real.
- Ruta: / → En este caso la ruta está vacía (solo “/”), lo que significa que estoy pidiendo la página principal por defecto.

Si escribo solo www.youtube.com en el navegador, este automáticamente asume el protocolo https:// y envía la solicitud al dominio. El servidor me redirige a la página de inicio de YouTube, que es su página por defecto (index principal). Esta página usualmente muestra recomendaciones y la interfaz principal.

**🧐✍️ Reporta en tu bitácora**

- Compara HTTP con los protocolos seriales que usaste.
- ¿Qué similitudes encuentras?
- ¿Qué diferencias clave ves?
- ¿Por qué crees que HTTP necesita ser más complejo que un simple envío de bytes como hacías con el micro:bit? 

R/ Similitudes

Tanto los protocolos seriales (como ASCII o binario con framing usados entre micro:bit y p5.js) como HTTP sirven para establecer reglas de comunicación claras entre dos entidades.

- En ambos casos hay un emisor (cliente) y un receptor (servidor) que deben entender el mismo “idioma” para que el mensaje tenga sentido.
- Ambos incluyen una estructura definida para los mensajes: en serial, se usaban delimitadores o framing para indicar inicio y fin; en HTTP, existen encabezados, métodos (GET, POST) y cuerpos de mensajes.
- En los dos protocolos, la comunicación implica una petición y una respuesta bien formadas. 

Diferencias clave

- Escala y complejidad:
Los protocolos seriales funcionan a nivel local entre dos dispositivos cercanos, mientras que HTTP opera a través de Internet, conectando navegadores con servidores distribuidos en todo el mundo.
- Formato de mensajes:
En serial, normalmente se envían cadenas simples o bytes sin mucha estructura. En HTTP, los mensajes incluyen encabezados, códigos de estado, información de tipo de contenido y rutas, lo que lo hace más detallado y organizado.
- Transporte:
Los protocolos seriales usan puertos físicos (USB/serial), mientras que HTTP viaja sobre protocolos de red (TCP/IP), a través de routers, cables y múltiples servidores intermedios.
- Propósito:
Serial se usa generalmente para control directo o intercambio de datos en tiempo real entre dispositivos. HTTP está diseñado para transferir recursos web (HTML, CSS, imágenes, APIs) entre navegadores y servidores, con soporte para múltiples tipos de contenido y operaciones. 

¿Por qué HTTP necesita ser más complejo?

HTTP debe soportar millones de clientes diferentes, distintos tipos de recursos y condiciones variables de red. A diferencia de un micro:bit conectado directamente al computador, en la web:

- Los mensajes deben incluir información adicional (tipo de archivo, estado, idioma, codificación, etc.) para que cualquier navegador pueda interpretar correctamente la respuesta.
HTTP debe funcionar de manera estandarizada y confiable entre sistemas heterogéneos (diferentes dispositivos, navegadores y servidores), no solo entre dos aparatos conocidos.
Además, debe garantizar seguridad, compatibilidad y extensibilidad, lo que requiere una estructura más compleja que el simple envío de bytes.

**🧐✍️ Reporta en tu bitácora**

Piensa en una página web simple, como un formulario de login.

¿Qué parte crees que es HTML (ej. los campos de texto, el botón)?
¿Qué parte es CSS (ej. el color del botón, el tipo de letra)?
¿Qué parte es JavaScript (ej. la comprobación de si escribiste algo antes de enviar, el mensaje de “contraseña incorrecta” que aparece sin recargar la página)?

R/ Imaginemos una página básica con un formulario de inicio de sesión que tiene:

- Un campo para usuario
- Un campo para contraseña
- Un botón de “Iniciar sesión”
- Un mensaje de error que aparece si los datos no son correctos

👉 HTML:

- Define la estructura: <form>, <input type="text">, <input type="password">, <button>.
- Es como el esqueleto de la página: indica que existen campos de texto, etiquetas y el botón, pero sin estilo ni lógica.

👉 CSS:

- Se encarga de la apariencia: 

  - Colores y degradados del botón.
  - Tipografía usada en las etiquetas y campos.
  - Tamaño y márgenes de los elementos.
  - Fondo de la página o bordes redondeados.

- Ejemplo: button { background-color: blue; font-family: Arial; border-radius: 5px; }

👉 JavaScript:

Añade la interacción:

  - Verifica que los campos no estén vacíos antes de enviar el formulario.
  - Muestra un mensaje de “contraseña incorrecta” en pantalla sin recargar la página.
  - Posiblemente también envía los datos al servidor usando AJAX/fetch, y actualiza el DOM según la respuesta.

Todo esto sucede gracias a funciones que se ejecutan en respuesta a eventos (por ejemplo, “click en el botón”). 

**🧐✍️ Reporta en tu bitácora**

**Compara el bucle draw() de p5.js con este modelo de “esperar a que algo pase y reaccionar”.**

R/ Bucle draw() de p5.js (imperativo):

- Se ejecuta constantemente, unas 60 veces por segundo.
- Ideal para animaciones, simulaciones o entornos gráficos interactivos que cambian constantemente (juegos, visualizaciones, etc.).
- El control está en el programador: define exactamente qué se repite y cómo.

👉 Modelo basado en eventos (JavaScript web):

- El código espera a que algo suceda (clic, envío de datos, cambio de ventana, llegada de un mensaje del servidor).
- Se registran “event listeners” que ejecutan funciones específicas cuando ocurre un evento.
- No hay un bucle constante redibujando todo: el navegador gestiona la detección de eventos de forma muy eficiente. 

**¿Qué ventajas crees que tiene el modelo basado en eventos para una interfaz de usuario web?**

R/ Ventajas del modelo basado en eventos para interfaces web

- Eficiencia: Solo se actualizan las partes necesarias de la página cuando algo cambia.
- Escalabilidad: Es más sencillo manejar muchas interacciones diferentes sin depender de un único bucle que controla todo.
- Rendimiento: Menor consumo de CPU y batería, ya que el navegador no está redibujando innecesariamente la interfaz a 60 FPS cuando nada ocurre. 
- Mejor organización del código: Se pueden separar las funciones por evento, lo que mejora la legibilidad y el mantenimiento.


**¿Sería eficiente tener un bucle draw() redibujando toda la página 60 veces por segundo si nada ha cambiado?**

No. Sería muy ineficiente, especialmente para sitios web complejos con muchos elementos. Redibujar constantemente una interfaz que no cambia genera consumo innecesario de recursos, provoca lag en dispositivos menos potentes y gasta más batería.
Por eso considero que el modelo basado en eventos es ideal para interfaces gráficas tradicionales ya que solo reaccionan cuando es necesario.

**🧐✍️ Reporta en tu bitácora**

**¿Por qué crees que podría ser útil usar JavaScript tanto en el cliente (navegador) como en el servidor? ¿Se te ocurre alguna ventaja para los desarrolladores?**

**R/** Según lo que investigué, usar JavaScript en ambos lados (cliente y servidor) es muy útil porque permite que todo el desarrollo se haga con un solo lenguaje, lo cual simplifica bastante el flujo de trabajo. Por ejemplo, un programador no tiene que aprender dos lenguajes diferentes ni cambiar su manera de pensar cuando pasa del frontend al backend. 

También esto facilita la reutilización de código, ya que se pueden compartir funciones, validaciones o estructuras entre ambas partes sin tener que reescribirlas. Otra ventaja clara es que los equipos pequeños pueden trabajar más rápido, porque no necesitan tener especialistas en varios lenguajes, y la comunicación entre frontend y backend se vuelve más fluida al “hablar el mismo idioma”. 

Me parece que esta unificación de lenguaje mejora bastante la eficiencia en el desarrollo, sobre todo en proyectos ágiles o con equipos reducidos, porque todo el mundo trabaja con las mismas bases y eso evita muchas confusiones.

**🧐✍️ Reporta en tu bitácora**

**Resume con tus propias palabras la diferencia fundamental entre una comunicación HTTP tradicional y una comunicación usando WebSockets/Socket.IO. ¿En qué tipo de aplicaciones has visto o podrías imaginar que se usa esta comunicación en tiempo real?** 

**R/** La diferencia fundamental entre la comunicación HTTP tradicional y WebSockets/Socket.IO es que:

- HTTP funciona como un intercambio de correos electrónicos: el cliente hace una petición y espera una respuesta del servidor. Si quiere seguir comunicándose, tiene que enviar otra petición cada vez.
- En cambio, WebSockets establecen una conexión directa y continua entre el cliente y el servidor, como si fuera una llamada telefónica. Con Socket.IO esta comunicación se vuelve mucho más sencilla, ya que permite enviar y recibir mensajes en tiempo real sin tener que abrir nuevas conexiones.

Este tipo de comunicación en tiempo real se usa, por ejemplo, en chats, videojuegos en línea, plataformas colaborativas (como Google Docs), sistemas de monitoreo o dashboards que muestran datos actualizados al instante. Básicamente, sirve para cualquier aplicación que necesite que la información fluya constantemente sin refrescar la página cada rato.

Yo entiendo que la gran ventaja de WebSockets es que permiten experiencias mucho más dinámicas e “instantáneas”, algo clave hoy en día para apps interactivas. HTTP sigue siendo útil, pero para comunicación continua, WebSockets marcan una gran diferencia.

Actividad 03

🧐🧪✍️ Experimenta

Detén el servidor si está corriendo.

Cambia la primera ruta de /page1 a /pagina_uno.

Inicia el servidor.

**Intenta acceder a http://localhost:3000/page1. ¿Funciona?**

<img width="1365" height="767" alt="Captura de pantalla 2025-10-03 134036" src="https://github.com/user-attachments/assets/629ae7b3-948e-4be5-8be5-28c335f1f80f" />

No, la página no funciona como se observa.

**Ahora intenta acceder a http://localhost:3000/pagina_uno. ¿Funciona?**

<img width="1365" height="767" alt="Captura de pantalla 2025-10-03 134046" src="https://github.com/user-attachments/assets/7e6acc90-72d9-46bb-aaef-b1dfd87e5a35" />

Sí, la página aparece sincronizando datos, es decir a que abra /page2 para completar el programa como antes de que modificara el programa.

¿Qué te dice esto sobre cómo el servidor asocia URLs con respuestas? Restaura el código.

R/ Esto me muestra claramente que el servidor asocia cada URL exactamente con la ruta que se define en el código, no “adivina” ni busca nombres parecidos. O sea, si cambio la URL en el servidor, también debo cambiar la URL a la que accedo desde el navegador.

En otras palabras, el servidor “escucha” rutas específicas, y si la petición no coincide exactamente con alguna, devuelve un error.

🧐🧪✍️
Experimenta

Asegúrate de que el servidor esté corriendo (npm start).

Abre http://localhost:3000/page1 en una pestaña. Observa la terminal del servidor. ¿Qué mensaje ves? Anota el ID.

<img width="581" height="371" alt="Captura de pantalla 2025-10-03 135555" src="https://github.com/user-attachments/assets/ba71f1c0-1f8f-41cf-9c44-77ba2dea5a0c" />

El ID que apareció es este: A user connected - ID: qWHp5rI4Eixh24hYAAAB. específicamente este: qWHp5rI4Eixh24hYAAAB.

Abre http://localhost:3000/page2 en OTRA pestaña. Observa la terminal. ¿Qué mensaje ves? ¿El ID es diferente?

<img width="582" height="371" alt="image" src="https://github.com/user-attachments/assets/6a82889b-d8e4-408c-a201-a1b46a012109" />

Me aparece el mensaje: A user connected - ID: eNXkcIhydOuN7g8sAAAF. Y sí el ID es diferente, se supone que es un ID único en cada caso.

Cierra la pestaña de page1. Observa la terminal. ¿Qué mensaje ves? ¿Coincide el ID con el que anotaste?

Me aparece este mensaje: User disconnected - ID: YHqpL8BaCLKQkI0hAAAD- Aparece con el ID con el que me conecté a /page1.

Cierra la pestaña de page2. Observa la terminal.

Me aparece este mensaje: User disconnected - ID: eNXkcIhydOuN7g8sAAAF. Y sí es el mismo ID con el que me conecté a /page2 por lo que confirma mi idea que son ID's únicos.

**Captura de la terminal cuando cierro ambas páginas:**

<img width="581" height="368" alt="image" src="https://github.com/user-attachments/assets/e3a7e361-ef0a-4d5b-a09f-438bb50fc093" />



🧐🧪✍️
Experimenta

Inicia el servidor y abre page1 y page2.

Mueve la ventana de page1. Observa la terminal del servidor. ¿Qué evento se registra (win1update o win2update)? ¿Qué datos (Data:) ves?

R/ 

https://github.com/user-attachments/assets/7273081d-c864-4004-892d-a969d6080f17

Si no se ve muy bien en la terminal se aprecia esto:

```
Received win1update from ID: WtX2_0XnbsI17k-0AAAH Data: { x: 603, y: 277, width:
 615, height: 656 }
Debug - Connected clients: 3, Page1: 2, Page2: 1, Synced: 3
All clients are fully synced
Debug - Connected clients: 3, Page1: 2, Page2: 1, Synced: 3
All clients are fully synced
Debug - Connected clients: 3, Page1: 2, Page2: 1, Synced: 3
All clients are fully synced
```

Y aparece "Received win1uptade" y yo supongo que el 1 es porque es la /page1 que se está moviendo.

Mueve la ventana de page2. Observa la terminal. ¿Qué evento se registra ahora? ¿Qué datos ves?

Se ve este mensaje:

```
Received win2update from ID: bR6WmCToZsSVjlmfAAAJ Data: { x: 186, y: 257, width: 1321, height: 656 }
Debug - Connected clients: 3, Page1: 2, Page2: 1, Synced: 3
All clients are fully synced
Debug - Connected clients: 3, Page1: 2, Page2: 1, Synced: 3
All clients are fully synced
Debug - Connected clients: 3, Page1: 2, Page2: 1, Synced: 3
All clients are fully synced
```

Se ve el mismo mensaje que moviendo la /page 1 solo que ahora es win2update por lo que se está refiriendo al movimiento de la /page 2.


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
