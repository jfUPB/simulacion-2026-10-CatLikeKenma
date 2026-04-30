# Unidad 7

## Bitácora de proceso de aprendizaje


## Bitácora de aplicación 
<img width="1995" height="1913" alt="Nota sin nombre2_1" src="https://github.com/user-attachments/assets/2384b713-29cf-4576-9f27-f67e107e85ad" />

```js
let font;
let particles = [];
let haunterPoints = [];
let state = "text"; 
let showTongue = false;
let tongueSound; 

let uStart, uEnd; 

function preload() {
  font = loadFont('PlayfulTime-BLBB8.ttf');
  tongueSound = loadSound('HaunterSound.mp3'); 
}

function setup() {
  // TRUCO CSS: Quitamos los márgenes del body para que la pantalla completa sea perfecta
  let css = document.createElement("style");
  css.innerHTML = "body { margin: 0; padding: 0; overflow: hidden; background-color: #0f0519; } canvas { display: block; }";
  document.head.appendChild(css);

  // Inicializamos el canvas al tamaño completo de la ventana
  createCanvas(windowWidth, windowHeight);

  let fullText = "HAUNTER";
  let fontSize = 140;
  
  // CENTRADO PERFECTO: Medimos el ancho y alto real de la palabra
  let bounds = font.textBounds(fullText, 0, 0, fontSize);
  let textX = width / 2 - bounds.w / 2;
  let textY = height / 2 + bounds.h / 2;

  let pts = font.textToPoints(fullText, textX, textY, fontSize, {
    sampleFactor: 0.35 
  });

  uStart = floor(pts.length * 0.28);
  uEnd = floor(pts.length * 0.42);

  let bodyPtsCount = pts.length - (uEnd - uStart);
  haunterPoints = generateHaunterShape(bodyPtsCount);
  let smilePts = generateSmilePoints(uEnd - uStart);

  let bodyIndex = 0; 
  for (let i = 0; i < pts.length; i++) {
    let targetX, targetY;
    
    if (i >= uStart && i < uEnd) {
      let sPt = smilePts[i - uStart];
      targetX = sPt.x;
      targetY = sPt.y;
    } else {
      targetX = haunterPoints[bodyIndex].x;
      targetY = haunterPoints[bodyIndex].y;
      bodyIndex++; 
    }
    
    particles.push(new Particle(pts[i].x, pts[i].y, targetX, targetY));
  }
}

function draw() {
  background(15, 5, 25, state === "transforming" ? 30 : 255);

  for (let p of particles) {
    p.update();
    p.show();
  }

  if (state === "transforming") {
    if (showTongue) drawTongue();
    drawHaunterFace();
    drawHaunterHands();
  }
}

function keyPressed() {
  if (key === 'q' || key === 'Q') {
    state = "transforming";
    if (getAudioContext().state !== 'running') {
      userStartAudio(); 
    }
  }

  if (key === ' ') {
    showTongue = !showTongue;
    if (showTongue && tongueSound) {
      tongueSound.play();
    } else if (tongueSound) {
      tongueSound.stop();
    }
  }
  
  if (key === 'f' || key === 'F') {
    let fs = fullscreen();
    fullscreen(!fs);
  }
}

function windowResized() {
  resizeCanvas(windowWidth, windowHeight);
  recalculateTargets();
}

function recalculateTargets() {
  // 1. Recalculamos el centro exacto para el texto
  let fullText = "HAUNTER";
  let fontSize = 140;
  let bounds = font.textBounds(fullText, 0, 0, fontSize);
  let textX = width / 2 - bounds.w / 2;
  let textY = height / 2 + bounds.h / 2;

  let pts = font.textToPoints(fullText, textX, textY, fontSize, {
    sampleFactor: 0.35 
  });

  // 2. Recalculamos los objetivos para la forma de Haunter
  let bodyPtsCount = particles.length - (uEnd - uStart);
  haunterPoints = generateHaunterShape(bodyPtsCount);
  let smilePts = generateSmilePoints(uEnd - uStart);

  let bodyIndex = 0;
  for (let i = 0; i < particles.length; i++) {
    
    // CORRECCIÓN: Si todavía es texto, movemos las partículas a su nuevo centro
    if (state === "text" && i < pts.length) {
      particles[i].pos.x = pts[i].x;
      particles[i].pos.y = pts[i].y;
    }

    if (i >= uStart && i < uEnd) {
      particles[i].target.x = smilePts[i - uStart].x;
      particles[i].target.y = smilePts[i - uStart].y;
    } else {
      particles[i].target.x = haunterPoints[bodyIndex].x;
      particles[i].target.y = haunterPoints[bodyIndex].y;
      bodyIndex++;
    }
  }
}

function generateSmilePoints(numPts) {
  let smile = [];
  for (let i = 0; i < numPts; i++) {
    let pct = i / numPts;
    let angle = map(pct, 0, 1, 0.2, PI - 0.2);
    let x = width / 2 + cos(angle) * 70; 
    let y = height / 2 + 20 + sin(angle) * 35; 
    smile.push(createVector(x, y));
  }
  return smile;
}

class Particle {
  constructor(x, y, tx, ty) {
    this.pos = createVector(x, y);
    this.target = createVector(tx, ty);
    this.vel = createVector(); 
    this.acc = createVector();
    this.maxSpeed = 8;
    this.maxForce = 0.5;
  }

  update() {
    if (state === "transforming") {
      let desired = p5.Vector.sub(this.target, this.pos);
      let d = desired.mag();
      let speed = d < 100 ? map(d, 0, 100, 0, this.maxSpeed) : this.maxSpeed;
      desired.setMag(speed);
      let steer = p5.Vector.sub(desired, this.vel);
      steer.limit(this.maxForce);
      this.applyForce(steer);
    }
    this.vel.add(this.acc);
    this.pos.add(this.vel);
    this.acc.mult(0);
  }

  applyForce(f) { this.acc.add(f); }

  show() {
    stroke(180, 100, 255, 180);
    strokeWeight(2.5);
    point(this.pos.x, this.pos.y);
  }
}

function generateHaunterShape(numPoints) {
  let pts = [];
  let cx = width / 2;
  let cy = height / 2;
  
  let silhouette = [
    createVector(0, -150), createVector(40, -130), createVector(110, -170),
    createVector(150, -60), createVector(140, 40), createVector(0, 220),
    createVector(-140, 40), createVector(-150, -60), createVector(-110, -170),
    createVector(-40, -130)
  ];
  
  let pointsPerSegment = floor(numPoints / silhouette.length);
  for (let i = 0; i < silhouette.length; i++) {
    let start = silhouette[i];
    let end = silhouette[(i + 1) % silhouette.length];
    for (let j = 0; j < pointsPerSegment; j++) {
      let pct = j / pointsPerSegment;
      pts.push(createVector(lerp(start.x, end.x, pct) + cx, lerp(start.y, end.y, pct) + cy));
    }
  }
  while (pts.length < numPoints) pts.push(createVector(cx, cy + 220));
  return pts;
}

function drawHaunterFace() {
  push();
  translate(width / 2, height / 2 - 20);
  fill(255); noStroke();
  beginShape(); vertex(-65, -30); vertex(-15, -10); vertex(-60, 5); endShape(CLOSE);
  beginShape(); vertex(65, -30); vertex(15, -10); vertex(60, 5); endShape(CLOSE);
  pop();
}

function drawHaunterHands() {
  let cx = width / 2; let cy = height / 2;
  let fY = sin(frameCount * 0.08) * 15;
  let fX = cos(frameCount * 0.05) * 10;
  drawClaw(cx - 230 + fX, cy + 100 + fY, -1);
  drawClaw(cx + 230 - fX, cy + 100 + fY, 1);
}

function drawClaw(x, y, side) {
  push();
  translate(x, y); scale(side, 1); rotate(PI / 4);
  stroke(200, 150, 255, 180); strokeWeight(2); fill(15, 5, 25, 200);
  beginShape();
  vertex(0, 0); vertex(20, -30); vertex(10, -5); vertex(45, 0);
  vertex(10, 5); vertex(20, 30); vertex(0, 10); vertex(-15, 0);
  endShape(CLOSE);
  pop();
}

function drawTongue() {
  push();
  translate(width / 2, height / 2 + 55); 
  fill(255, 50, 100); noStroke();
  let swing = sin(frameCount * 0.15) * 12;
  beginShape();
  vertex(-15, 0); 
  bezierVertex(-25, 40, -35 + swing, 110, 0 + swing, 130); 
  bezierVertex(35 + swing, 110, 25, 40, 15, 0); 
  endShape(CLOSE);
  stroke(200, 30, 80); strokeWeight(2);
  line(0 + swing/2, 15, 0 + swing, 100);
  pop();
}
```
[Link Edit:](https://editor.p5js.org/CatLikeKenma/sketches/HXhcqW-_L)
[Link View:](https://editor.p5js.org/CatLikeKenma/full/HXhcqW-_L)

<img width="609" height="378" alt="Captura de pantalla 2026-04-30 134920" src="https://github.com/user-attachments/assets/964875f9-6d1a-4876-bd55-e67c2d032c17" />
<img width="584" height="392" alt="Captura de pantalla 2026-04-30 134939" src="https://github.com/user-attachments/assets/6335bb80-44ec-4540-a8ea-6112351fe271" />

## Bitácora de reflexión
