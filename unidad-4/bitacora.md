# Unidad 4

## Bitácora de proceso de aprendizaje
**Actividad 01**
1. Me gusta lo psicodélica que se ve esta obra, al inicio con estas conexiones tipo constelaciones, después esta maya tipo agua, le da un efecto 3D muy imapactante, al añadirle las luces me recuerda al tipo de efectos y shows presentados en los raves o conciertos de tecno, tipo tomorrowland, que te llevan a otro mundo. Es una obra que te mantiene enfocado, no pierdes la atención de ella.

**Actividad 02**
1. Un objeto gira alrededor en el centro del canvas. La interacción consiste en que al apretar una tecla el ángulo de rotación aumenta.
2. 

**Actividad 03**
1.
```js
let pos;
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
```
**Actividad 04**
1.

## Bitácora de aplicación 
1. Mi obra quise hacerla esta vez basada en una galaxia, la anterior, fue basada más en el agua, pues me genera tranquilidad, sin mebargo, ultimamente he estado pintando mucho, y he pensado mucho en el color. Lo primero que se me vino a la cabeza fue el espacio, y tras algunos intentos fallidos de hacer una interacción tipo el juego "Asteroids" me decanté por esta galaxia. También siento que representa un poco cómo funciona mi cerebro a veces... yendo lento, o muchísimo más rápido de lo que debería. Me gustó mucho el resultado.
  
3.
```js
   let stars = [];

function setup() {
  createCanvas(640,480);
  background(5,10,30);

  for(let i=0;i<120;i++){
    stars.push(new Star());
  }
}

function draw() {

  fill(5,10,30,25);
  noStroke();
  rect(0,0,width,height);

  translate(width/2,height/2);

  let waveAmp = map(mouseX,0,width,10,120);
  let speedControl = map(mouseY,0,height,0.002,0.04);

  for(let s of stars){
    s.update(waveAmp,speedControl);
    s.display();
  }

}

class Star{

  constructor(){

    this.theta = random(TWO_PI);
    this.baseR = random(50,200);

    this.pos = createVector(0,0);
    this.vel = createVector(0,0);

    this.size = random(2,4);

  }

  update(waveAmp,speed){

    this.theta += speed;

    let wave = sin(frameCount*0.04 + this.theta*2);

    let r = this.baseR + wave*waveAmp;

    let newPos = p5.Vector.fromAngle(this.theta,r);

    this.vel = p5.Vector.sub(newPos,this.pos);
    this.pos = newPos.copy();

  }

  display(){

    push();

    translate(this.pos.x,this.pos.y);

    rotate(this.vel.heading());

    let c = map(this.baseR,50,200,180,255);

    stroke(c,200,255,160);
    fill(c,200,255,140);

    ellipse(0,0,this.size);

    pop();

  }

}

function mousePressed(){

  for(let i=0;i<30;i++){
    stars.push(new Star());
  }

}

function keyPressed(){

  if(key === 'c'){
    background(5,10,30);
  }

}
```
3. https://editor.p5js.org/CatLikeKenma/sketches/ItsEUqmT6
4. 
<img width="612" height="456" alt="Captura de pantalla 2026-03-16 232549" src="https://github.com/user-attachments/assets/22bce372-96f0-4677-8628-93590c0c648e" />

## Bitácora de reflexión

