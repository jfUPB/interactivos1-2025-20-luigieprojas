# Auto evaluación Unidad 5

**Autoevaluación — Unidad 5:** 

En esta unidad diseñé, implementé y verifiqué un protocolo de comunicación binario entre un micro:bit y un sketch en p5.js. Realicé experimentos comparativos (ASCII vs binario), añadí framing y checksum, detecté y resolví problemas de sincronización, documenté resultados con capturas y propuse pruebas de robustez. Considero que el trabajo demuestra un nivel de dominio avanzado con algunos retos pendientes menores, por eso propongo una nota final de 4.7 / 5.0. A continuación explico criterio por criterio.

**1. Profundidad de la indagación — Calificación propuesta: 4.8 / 5.0**

**Justificación:**

Considero que he demostrado preguntas de indagación que van más allá del “cómo” del código y tocan el “porqué” del diseño del protocolo: comparé ASCII vs binario, discutí trade-offs (legibilidad vs eficiencia), planteé framing y checksum como soluciones a la desincronización y analicé el impacto del endianness y el tamaño fijo de paquete en la robustez. Además propuse experimentos que responden esas preguntas (ver más abajo). No es 5.0 puro porque quedaron retos sin terminar por falta de tiempo (mencionado en la bitácora), pero la reflexión y las preguntas son de nivel excelente.

**Evidencias en la bitácora:**

Explicación comparativa explícita entre ASCII y binario (ventajas/desventajas). Ejemplo de reflexión: “La elección de un protocolo tan sencillo como ASCII es más costosa en bytes transmitidos, pero muy práctica para depuración y aprendizaje.” (Actividad 01).
Captura: [imagen ASCII](https://github.com/user-attachments/assets/c9b60969-bd76-43be-8d09-a37a801d8818)
.

Código y experimento que muestra struct.pack('>2h2B', xValue, yValue, int(aState), int(bState)) y el análisis de por qué producen 6 bytes por paquete (Actividad 02).
Captura: [resultado en binario](https://github.com/user-attachments/assets/c9de5de0-6337-4be3-bb87-6ed0d02e373b)
.

**Preguntas planteadas en la bitácora:** “¿Qué pasaría si se omitiera el \n?”, “¿Qué ventajas/desventajas en ASCII vs binario?”, y la consideración de framing/checksum como soluciones.

**Comentario final del criterio:** la indagación es profunda, plantea hipótesis y contrasta opciones; faltó completar algunos experimentos adicionales que sugerí, por eso 4.8 en vez de 5.0.

**2. Calidad de la experimentación — Calificación propuesta: 4.6 / 5.0**

**Justificación:**

Considero que mis experimentos fueron bien diseñados y muestran control: comparé la salida ASCII legible con la salida binaria en Hex, cuantifiqué el tamaño (6 bytes por paquete) y añadí medidas de verificación (checksum). Implementé en el micro:bit el envío binario con framing (0xAA) y checksum, y en p5.js hice el parsing con DataView y verificación de checksum. Asimismo añadí simuladores (inyección de basura, modo high-rate) para probar robustez. La razón para no dar 5.0 es la falta de algunos experimentos adicionales replicando varias condiciones de fallo en hardware físico (aunque sí hay simulaciones y pruebas reproducibles).

**Evidencias en la bitácora:**

Micro:bit (emisor) que usa struct.pack y luego envía packet = b'\xAA' + data + bytes([checksum]) (Actividad 04).
Código: [fragmento micro:bit](https://github.com/user-attachments/assets/937b6a5c-bb98-430b-95a9-0984381c0dc7)
.

Capturas y explicación del resultado Hex: “FF EC 00 3A 01 00” y su correspondencia con -20, 58, 1, 0 (Actividad 02).
Captura: [resultado Hex](https://github.com/user-attachments/assets/3130a4d1-7cd4-44f9-a739-066b055159d7)
.

Sketch p5.js modificado con readLoop() y DataView para decodificar big-endian y validar checksum, incluyendo el código que procesa el buffer y busca 0xAA (Actividad 04).
Código: [readLoop p5.js](https://github.com/user-attachments/assets/23701393-f164-46bf-b0d7-bdee5ea07557)
.

**Tests de robustez:** simulador de alta tasa, inyección de basura y simulación de paquetes corruptos (mi versión extendida de p5.js).
Evidencia: [paquetes descartados por checksum](https://github.com/user-attachments/assets/ee4883bd-a714-4d22-93bc-f49fb313ca14)
.

**Comentario final del criterio:** considero que los experimentos fueron excelentes, con pruebas de robustez y simulaciones. Si hubiera replicado más pruebas físicas en distintos entornos (por ejemplo, distintas tasas de baud o cables largos), hubiese sido 5.0; por eso 4.6.

**3. Análisis y reflexión — Calificación propuesta: 4.7 / 5.0**

**Justificación:**

En mi bitácora conecté claramente los datos experimentales con las conclusiones: identifiqué la causa raíz de errores (desalineación del buffer), propuse framing + checksum como solución, mostré cómo la sincronización previa producía valores erróneos y cómo la solución robustecía el sistema. Además documenté errores de desarrollo (por ejemplo ReferenceError: connectBtnClick is not defined) y cómo los corregí. La reflexión incluye trade-offs (legibilidad vs eficiencia) y consideraciones de diseño (dónde conviene detectar eventos — emisor vs receptor). Falta una modelización matemática más formal del error de sincronización (no estrictamente necesaria, pero hubiera elevado a 5.0).

**Evidencias en la bitácora:**

**Registro de error en consola:** la lectura con readBytes(6) a veces tomaba bytes a mitad de paquete → valores desplazados.
Captura: [error de sincronización](https://github.com/user-attachments/assets/3130a4d1-7cd4-44f9-a739-066b055159d7)
.

**Solución implementada y resultado:** implementación de framing (0xAA), verificación checksum y evidencia de que la consola deja de mostrar valores erráticos.
Capturas: [sincronización corregida 1](https://github.com/user-attachments/assets/23701393-f164-46bf-b0d7-bdee5ea07557)
 y [sincronización corregida 2](https://github.com/user-attachments/assets/e7d1cbf0-bcf7-4a75-8bc8-a601ef0d0d5b)
.

Documentación de errores de desarrollo y cómo se corrigieron: ejemplo ReferenceError: connectBtnClick is not defined y solución propuesta para asignar isConnected = true en connectBtnClick().

**Reflexiones técnicas:** discusión sobre complemento a dos, endianess, y cómo la elección de short (h) limita y define el rango de acelerómetro.

**Comentario final del criterio:** considero que mi análisis está bien conectado a la evidencia experimental y a la corrección de errores; queda espacio para una modelización cuantitativa más formal, de ahí 4.7.

**4. Apropiación y articulación de conceptos — Calificación propuesta: 4.7 / 5.0**

**Justificación:**

Considero que he demostrado comprensión clara de los conceptos centrales: framing, checksum, empaquetado binario (struct.pack), DataView en JS, detección de flancos para eventos (A pressed / B released), y la distinción entre enviar estados vs enviar eventos. Los conceptos están articulados y aplicados al sistema completo (micro:bit → puerto serie → p5.js). No es 5.0 por pequeños detalles de formalización (por ejemplo, una sola sección con un diagrama formal de estado/protocolo — aunque la lógica está bien descrita en el código).

**Evidencias en la bitácora:**

Empaquetado en micro:bit: struct.pack('>2h2B', ...), checksum y b'\xAA' como cabecera (Actividad 04).
Código: [paquete micro:bit](https://github.com/user-attachments/assets/937b6a5c-bb98-430b-95a9-0984381c0dc7)
.

**Receptor en p5.js:** uso de DataView y getInt16/getUint8 con big-endian, y código para validar checksum antes de actualizar variables (fragmento del readLoop()).
Captura: [validación checksum](https://github.com/user-attachments/assets/e7d1cbf0-bcf7-4a75-8bc8-a601ef0d0d5b)
.

**Detección de flancos y generación de eventos en p5.js:** función updateButtonStates(newAState, newBState) que detecta A pressed y B released.

**Explicación conceptual de trade-offs:** “ASCII es más legible para debugging; binario es más eficiente y predecible” (Actividad 01/02).

**Código que construye paquete y suma checksum:** buildPacket(x, y, a, b) (mi versión de utilidades en p5.js), y simuladores que prueban condiciones de borde.

**Comentario final del criterio:** considero que he demostrado dominio conceptual claro y aplicado. Faltó una pequeña pieza documental formal (diagrama o esquema de estado/protocolo) para poder llegar al 5.0 sin matices; por eso 4.7.

**Nota final y cálculo**

**Calificaciones por criterio propuestas:**

- **Profundidad de la Indagación:** 4.8
- **Calidad de la Experimentación:** 4.6
- **Análisis y Reflexión:** 4.7
- **Apropiación y Articulación de Conceptos:** 4.7

**Cálculo de la nota final:**

- **Suma:** 4.8 + 4.6 + 4.7 + 4.7 = 18.8
- **Promedio:** 18.8 / 4 = 4.7
- **Nota:** 4.7


