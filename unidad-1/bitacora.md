# Unidad 1

## Bitácora de proceso de aprendizaje
**Actividad 1**
1. Considero que la aleatoriedad puede asemejar más un diseño a la realidad, pues el mundo que conocemos está lleno de imperfección, y esto lo hace mucho más interesante.

**Actividad 2**
1. ¿Qué espero que suceda? No esperé ningún cambio significativo. Pues aún no entendía como funcionaban los ejes del canvas.
2. No ocurrió lo que esperaba.

**Actividad 3**
1. Una distribución uniforme no tiene tanta aleatoriedad, por lo que no hay tantos saltos, el color, en este caso, se enfoca en un área específica. La distribución no uniforme tiene muchísimos valores aleatorios, por lo que el color podría dividirse y aparecer en muchísimas zonas diferentes, con la posibilidad de ocupar todo el lienzo.

**Actividad 4**
1.
function setup() {
  createCanvas(640, 400);
  background(255);
}

function draw() {
  let meanRadius = 80;   
  let sd = 15;           

  
  let r = randomGaussian(meanRadius, sd);

 
  let angle = random(TWO_PI);

  
  let x = width / 2 + r * cos(angle);
  let y = height / 2 + r * sin(angle);

  noStroke();
  fill(0, 10);
  circle(x, y, 16);
}

(Hice un círculo en lugar de una línea)
<img width="371" height="298" alt="Captura de pantalla 2026-01-28 164143" src="https://github.com/user-attachments/assets/99bd9350-d796-478a-a6d2-92e727f1ab97" />

**Actividad 5**
1. Utilicé esta técnica porque así quería evitar aún más que el trazo fuese predecible, añadiendo algunos pasos pequeños, saltos muy grandes en el lienzo, y también evitar que se concentre todo en el centro como una masa densa. (Al final esta masa densa si se formó, pero tardó mucho más tiempo en hacerlo)
2.
let x;

function setup() {
  createCanvas(640, 240);
  background(255);
  x = width / 2;
}

let pos;

function setup() {
  createCanvas(640, 400);
  background(255);
  pos = createVector(width / 2, height / 2);
}

function draw() {

  let targetX = randomGaussian(width / 2, 80);
  let targetY = randomGaussian(height / 2, 80);
  let target = createVector(targetX, targetY);


  let dir = p5.Vector.sub(target, pos);
  dir.normalize();


  let step = levy();

  dir.mult(step);
  pos.add(dir);


  pos.x = constrain(pos.x, 0, width);
  pos.y = constrain(pos.y, 0, height);


  noStroke();
  fill(0, 15);
  circle(pos.x, pos.y, 8);
}



function levy() {
  while (true) {
    let r1 = random(1);
    let probability = 1 / pow(r1, 1.5);
    probability = constrain(probability, 0, 1);

    let r2 = random(1);

    if (r2 < probability) {
      return r1 * 40; 
    }
  }
}

3. https://editor.p5js.org/CatLikeKenma/full/WPoos9OwT
4. <img width="332" height="279" alt="Captura de pantalla 2026-01-28 171348" src="https://github.com/user-attachments/assets/00c4877d-b126-4a31-b583-999418e59a83" />

**Actividad 6**
1. Quería crear algo tan fluido como el ejemplo del agua en clase, pero decidí hacer algo tipo trazos de tinta. Algo con una dirección, no solo varie la altura.
2. 
let particles = [];
let t = 0;

function setup() {
  createCanvas(360, 240);
  background(255);

  // Crear muchas partículas
  for (let i = 0; i < 400; i++) {
    particles.push(createVector(random(width), random(height)));
  }
}

function draw() {
  noStroke();
  fill(255, 10);
  rect(0, 0, width, height);

  stroke(0, 40);
  strokeWeight(1);

  for (let p of particles) {

    
    let angle = noise(p.x * 0.005, p.y * 0.005, t) * TWO_PI * 2;

    let v = p5.Vector.fromAngle(angle);
    v.setMag(1);

    let prev = p.copy();
    p.add(v);

    line(prev.x, prev.y, p.x, p.y);

    if (p.x < 0) p.x = width;
    if (p.x > width) p.x = 0;
    if (p.y < 0) p.y = height;
    if (p.y > height) p.y = 0;
  }

  t += 0.003;
}

3.https://editor.p5js.org/CatLikeKenma/full/GF3hwEQJO

4. <img width="292" height="190" alt="Captura de pantalla 2026-01-28 181402" src="https://github.com/user-attachments/assets/03aa7513-b12b-4eaa-9426-69e69057a208" />


## Bitácora de aplicación 



## Bitácora de reflexión


