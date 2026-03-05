# Unidad 4

## Bitácora de proceso de aprendizaje
**Actividad 01**
1. Me gusta lo psicodélica que se ve esta obra, al inicio con estas conexiones tipo constelaciones, después esta maya tipo agua, le da un efecto 3D muy imapactante, al añadirle las luces me recuerda al tipo de efectos y shows presentados en los raves o conciertos de tecno, tipo tomorrowland, que te llevan a otro mundo. Es una obra que te mantiene enfocado, no pierdes la atención de ella.

**Actividad 02**


**Actividad 03**
1. let pos;
let vel;

function setup() {
  createCanvas(600, 400);
  pos = createVector(width/2, height/2);
  vel = createVector(0, 0);
}

function draw() {
  background(220);

  if (keyIsDown(LEFT_ARROW)) {
    vel.x -= 0.1; // acelera hacia la izquierda
  }
  
  if (keyIsDown(RIGHT_ARROW)) {
    vel.x += 0.1; // acelera hacia la derecha
  }

  pos.add(vel);

  vel.mult(0.98);

  push();
  translate(pos.x, pos.y);

  let angle = vel.heading();
  rotate(angle);

  fill(50,150,255);
  noStroke();

  triangle(-15, -10, -15, 10, 15, 0);

  pop();
}

**Actividad 04**
1.

## Bitácora de aplicación 



## Bitácora de reflexión
