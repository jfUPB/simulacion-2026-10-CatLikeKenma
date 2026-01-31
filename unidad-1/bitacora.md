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

1. 
let particles = [];
let t = 0;

function setup() {
  createCanvas(600, 400);
  background(255);
  colorMode(HSB, 360, 100, 100, 100);
}

function draw() {
  noStroke();
  fill(255, 6);
  rect(0, 0, width, height);

  if (mouseIsPressed) {
    for (let i = 0; i < 4; i++) {
      particles.push(new Particle(mouseX, mouseY));
    }
  }

  for (let p of particles) {
    p.update();
  }

  t += 0.003;
}


class Particle {
  constructor(x, y) {
    this.pos = createVector(x, y);
    this.hue = random(360);
  }

  update() {
    let steps = 8; 

    for (let i = 0; i < steps; i++) {


      let gx = randomGaussian(mouseX, 40);
      let gy = randomGaussian(mouseY, 40);
      let target = createVector(gx, gy);

      let dir = p5.Vector.sub(target, this.pos);

      let angle = noise(this.pos.x * 0.004, this.pos.y * 0.004, t) * TWO_PI * 2;
      let flow = p5.Vector.fromAngle(angle);

      dir.add(flow).normalize();

      let step = levy() / steps;
      dir.mult(step);

      let prev = this.pos.copy();
      this.pos.add(dir);


      stroke(this.hue, 80, 80, 35);
      strokeWeight(1.2);
      line(prev.x, prev.y, this.pos.x, this.pos.y);
    }
  }
}



function levy() {
  while (true) {
    let r1 = random(1);
    let probability = 1 / pow(r1, 1.5);
    probability = constrain(probability, 0, 1);
    let r2 = random(1);

    if (r2 < probability) {
      return r1 * 18;
    }
  }
}

2. https://editor.p5js.org/CatLikeKenma/full/YqPLMCoVc
3. <img width="382" height="262" alt="Captura de pantalla 2026-01-31 111049" src="https://github.com/user-attachments/assets/b123f28f-1d0b-4964-ac75-4ba29e7d7585" />

## Bitácora de reflexión

1. La diferencia es que la aleatoriedad de ambas no es la misma. El random tiene unos cambios más bruscos y extremos, mientras el Perlin tiene unos cambios más suaves, que tiene una correlación con los valores anteriores. El primero lo usaría para un contexto donde los cambios extremos sean necesarios, sin una continuidad, como un diagrama de líneas por ejemplo, y el perlin lo usaría cuando necesite una figura más orgánica, que se vea más viva, con una fluidez.
2. Una distribución de probabilidad es la forma en la que se reparten las posibilidades de que algo ocurra. Una caminata con distribución uniforme puede distribuirse por todo un espacio, no tiene un centro o un enfoque, y se esparce de forma pareja; la distribución normal, se concentra en una zona específica o tiene una órbita, puede verse un centro más denso y los puntos no se alejan mucho en el espacio.
3. La aleatoriedad permite que la obra sea poco predecible. Genera variación y tamién puede simular elementos naturales.
4. El ruido perlin en mi último ejercicio ayudó a que la dirección que tomaran las líneas fueran en una forma curva y fluida, que se vea ese efecto de tinta que se desplaza continuamente.
5. Un walk es la forma en la que puede estar paso a paso, donde la nueva posición depende de la anterior. Una caminata tipo Levy Flight, en lugar de tener siempre pasos parecidos como una normal, tiene la peculiaridad de que a veces, da unos saltos largos.


