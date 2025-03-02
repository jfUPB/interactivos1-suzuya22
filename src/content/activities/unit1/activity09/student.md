### programa en p5.js que genere patrones visuales aleatorios   

``` js
function setup() {
  createCanvas(800, 800);
  noLoop();
  background(255); 
}

function draw() {
  let cols = 20;
  let rows = 20; 
  let cellWidth = width / cols; 
  let cellHeight = height / rows; 

  for (let i = 0; i < cols; i++) {
    for (let j = 0; j < rows; j++) {
      let x = i * cellWidth + cellWidth / 2;
      let y = j * cellHeight + cellHeight / 2; 

      let circleSize = (sin(frameCount * 0.01 + i * j) * 0.5 + 0.5) * cellWidth * 0.8;

      let r = random(100, 255);
      let g = random(100, 255);
      let b = random(100, 255);

      fill(r, g, b, 150); 
      noStroke(); 
      ellipse(x, y, circleSize, circleSize); 
    }
  }
}

function mousePressed() {
  redraw(); 
}
```
Lo que hace el programa es primero mediante un "noLoop" evitar que se generen infinitamente las figuras,  despues se determina un fondo blanco mediante el "background(255);" para luego pasar a dibujar las figuras con la 
function draw  

#### Como funciona la function draw?
primero se determinan las filas y hileras de la cuadricula, en este caso son 20 y 20
``` js
 let cols = 20;
  let rows = 20;
```
despues se determina el ancho y largo de estas mismas
``` js
  let cellWidth = width / cols; 
  let cellHeight = height / rows;
```
con las celdas creadas se da paso a un ciclo for donde se recorren cada una de ellas para determinar cuantas hay
se empieza con una variable i que aumenta de a 1 siempre y cuando sea menor al numero de celdas (20)
``` js
 for (let i = 0; i < cols; i++)
```
despues se ubica cada celda y se posiciona en el centro de esta con:
``` js
let x = i * cellWidth + cellWidth / 2;
```
avanzan horizontal y verticalmente mediante i y j


por ultimo se generan circulos mediante la funcion seno y se definen colores aleatorios para estos mismos con:
``` js
  let circleSize = (sin(frameCount * 0.01 + i * j) * 0.5 + 0.5) * cellWidth * 0.8;

     
      let r = random(100, 255);
      let g = random(100, 255);
      let b = random(100, 255);

      fill(r, g, b, 150); 
      noStroke(); 
      ellipse(x, y, circleSize, circleSize);
```
### Patron generado
![image](https://github.com/jfUPB/interactivos1-suzuya22/blob/main/src/assets/captura%20actividad%209.png)
