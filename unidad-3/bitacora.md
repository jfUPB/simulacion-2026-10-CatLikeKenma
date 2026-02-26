# Unidad 3

## Bitácora de proceso de aprendizaje
**Actividad 03**
1. Resistencia del aire: https://editor.p5js.org/CatLikeKenma/sketches/LM4sFYf3H
2. Atracción gravitatoria: https://editor.p5js.org/CatLikeKenma/sketches/Trbn5ET7Y
3. Fricción: https://editor.p5js.org/CatLikeKenma/sketches/BC8O_ft11

## Bitácora de aplicación 
**Actividad 04**
1. En esta obra quise acercarme a una temática un poco más tipo acuática, porque en estos días no me he sentido muy bien y el agua es algo que puede calmarme. Decidí hacer una tipo maya, donde el mouse interactúa como un "avivador" dentro de esa calma. Me gusta porque de no moverse el mouse, parece una simple maya, pero al deformarse, es casi como si cobrara vida.

2. 
```js
let cols;
let rows;
let spacing = 15;
let grid = [];
let damping = 0.98;     
let tension = 0.02;     
let spread = 0.25;      

function setup() {
  createCanvas(800, 600);
  cols = width / spacing;
  rows = height / spacing;
  
  for (let x = 0; x < cols; x++) {
    grid[x] = [];
    for (let y = 0; y < rows; y++) {
      grid[x][y] = {
        baseY: y * spacing,
        yOffset: 0,
        velocity: 0
      };
    }
  }
}

function draw() {
  background(10, 20, 40);

  updatePhysics();
  displayWater();
}


function updatePhysics() {
  
  for (let x = 0; x < cols; x++) {
    for (let y = 0; y < rows; y++) {
      
      let cell = grid[x][y];
      
      let force = -tension * cell.yOffset;
      cell.velocity += force;
      cell.velocity *= damping;
      cell.yOffset += cell.velocity;
    }
  }

  for (let x = 1; x < cols - 1; x++) {
    for (let y = 1; y < rows - 1; y++) {
      
      let cell = grid[x][y];
      let sum = 0;
      
      sum += grid[x-1][y].yOffset;
      sum += grid[x+1][y].yOffset;
      sum += grid[x][y-1].yOffset;
      sum += grid[x][y+1].yOffset;
      
      let average = sum / 4;
      cell.velocity += (average - cell.yOffset) * spread;
    }
  }
}

function mouseMoved() {
  disturb(mouseX, mouseY, 5);
}

function mousePressed() {
  disturb(mouseX, mouseY, 15);
}

function disturb(mx, my, power) {
  for (let x = 0; x < cols; x++) {
    for (let y = 0; y < rows; y++) {
      
      let px = x * spacing;
      let py = y * spacing;
      
      let d = dist(mx, my, px, py);
      
      if (d < 50) {
        grid[x][y].velocity += power * (1 - d/50);
      }
    }
  }
}


function displayWater() {
  noFill();
  stroke(100, 150, 255, 120);
  
  for (let y = 0; y < rows; y++) {
    beginShape();
    for (let x = 0; x < cols; x++) {
      
      let px = x * spacing;
      let py = grid[x][y].baseY + grid[x][y].yOffset;
      
      vertex(px, py);
    }
    endShape();
  }
}
```
3. https://editor.p5js.org/CatLikeKenma/sketches/Trbn5ET7Y
4. <img width="731" height="584" alt="Captura de pantalla 2026-02-25 224723" src="https://github.com/user-attachments/assets/06c26abb-dfad-4828-94aa-ee3ea772f3c2" />

## Bitácora de reflexión


