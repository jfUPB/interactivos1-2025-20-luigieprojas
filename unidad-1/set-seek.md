# Unidad 1

## 🔎 Fase: Set + Seek

### Actividad 01

**1. ¿Qué es un sistema físico interactivo?**  
**R/**  Un sistema físico interactivo para mí es un programa que tiene inputs, procesamiento y ouputs, que como su nombre lo dicen interactuan con las acciones del usuario para generar un producto cambiante.  

**2. ¿Cómo podrías aplicar lo que has visto en tu perfil profesional?**  
**R/** Con esto podría mejorar mucho mis ilustraciones, hacer portadas de mis cómics con todos estos efectos de manera interactiva, algo que me parece muy novedoso. También con esto podría hacer ediciones de vídeo mucho más ricas en contenido y con un apego visual muy llamativo, se me ocurren muchas ideas para implementar estos sistemas en mis vídeos. Como también me gusta mucho la música y hacer mis canciones, podría promocionar mis temas con estos efectos visuales para darle un aspecto más profesional.

### Actividad 02  

**1. ¿Qué es el diseño/arte generativo?**  
**R/** El arte generativo es una práctica en la cual mediante un código/sistema y ciertas reglas propuestas por el creador, el plotter/máquina con cierta autonomía crea obras aleatoriamente generadas que siguen las normas y tienen cierta variedad ya preestablecida en el código.  

**2. ¿Cómo podrías aplicar lo que has visto en tu perfil profesional?**  
**R/** Podría aplicar esto que vi en muchas cosas, como por ejemplo en videoclips musicales haciendo un visualizer, tambien colocando elementos en el videoclip como el espectro del audio. También puedo aplicarlo diseñando páginas webs para mis proyectos (cómics, perfil profesional, etc.) y usarlo en mis ilustraciones para elevarlas a un nuevo nivel interactivo.  

### Actividad 03

**1. En este sistemas físico interactivo identifica los inputs, outputs y el proceso.**  
**R/** Outputs: Microbit, Los cambios de color en el Browser.
Input: acelerometro, el serial/cable que envía los datos al computador para mostrar las animaciones, los botones. 
Proceso: El proceso es el código que tienen los computadores/programa en javascript y python.

### Actividad 04  
[Este es el enlace a mi programa](https://editor.p5js.org/luigieprojas/sketches/0dpQVzKJf)

**Imagen generada**

<img width="402" height="437" alt="image" src="https://github.com/user-attachments/assets/91734607-a966-431d-9042-48360f5ea3d8" />

**Código**

```
function setup() {
  createCanvas(400, 400);
}

function draw() {
  background(mouseX,10);
  stroke(100);
  fill('blue');
  triangle(random(mouseX,mouseY),random(mouseX,mouseY),random(3,50),random(5,200), random(5,200),random(mouseX,50));
}
```
