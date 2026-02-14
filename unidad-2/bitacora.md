# Unidad 2

## Bitácora de proceso de aprendizaje
**Actividad 01**
1. Me gustó mucho la animación procedural, la criatura tipo cangrejo. Me llamó mmucho la atención su desplazamiento tan fluido, y me hace preguntarme las posibilidades que habría de implementar este tipo de animación y criaturas en proyectos de animación más grandes.

**Actividad 02**
1. Sumar vectores es diferente a una suma simple, pues debe tenerse en cuenta los elementos que compone a cada vector, por lo que se utiliza la función add. para sumar los omponentes (en este caso al vector position) y se modifica directamente.
2. No funciona porque estos dos conceptos no son números, o al menos, no se conciben como tal, sino como objetos. El + no permite sumar estos vectores, podría interpretarlo como tipo strings y no funcionaría bien.

**Actividad 03**
1. En lugar de pensar en coordenadas sueltas como i y Y, creé un vector con estas coordenadas para que funcione como tal. Ahora es posible ver la posición del walk como un objeto vectorial.
2.

let t = 0.0;

function setup() {
  createCanvas(360, 240);
}

function draw() {
  background(255);

  let xoff = t;

  noFill();
  stroke(0);
  strokeWeight(2);
  beginShape();

  for (let i = 0; i < width; i++) {
    let y = noise(xoff) * height;


  let position = createVector(i, y);

  vertex(position.x, position.y);

  xoff += 0.01;
  }

  endShape();
  t += 0.01;
}

**Actividad 4**
1. No sé muy bien qué esperar, pero veo que se crea un vector, y sus valores al parecer cambian en un punto.
2. El vector, que antes tenía los valores (6,9) ahora cambió a (20,30)
4. En este código se está realizando un paso por referencia, porque v y position están apuntando al mismo objeto, al mismo vector, y porque al ejecutar la función, el vector original cambia.
5. Recordé conceptos de programación que siendo honesta, no recordaba. Aprendí a referenciar un vector

**Actividad 5**
1. El método mag() devuelve la longitud de un vector, es usado para calcular la magnitud de un vector 2D. Se hace esto con la raíz de x^2 + y^2. El metodo magsq() hace lo mismo pero con la diferencia de que este último método no aplica la raíz en la ecuación y devuelve la longitud al cuadrado. Magsq() es más eficiente porque calcular la raíz cuadrada es computacionalmente más difícil que sumar y multiplicar.
2. El método normalize escala los componentes de un vector hasta que su magnitud sea 1, convirtiéndolo en un vector unitario.
3. El método dot() sirve para saber qué tan alineados están dos vectores, sin van hacia el mismo lado, en sentido perpendicular o en sentido contrario.
4. La diferencia es que la versión de instancia se llama desde un vector existente, y la versión estática llama desde la clase p5.vector. Sin embargo, el cálculo es el mismo.
5. El producto cruz entre dos vectores genera un nuevo vector que es perpendicular al plano que forman, su magnitud representa el área del paralelogramo entre ellos y su orientación depende del orden de su multiplicación.
6. El método dist() puede utilizarse para detectar colisiones, saber si el mouse está cerca a un objeto, activar interacciones por proximidad y medir distancia entre partículas.
7. Normalize() convierte los vectores en vectores unitarios, y limit() se usa para restringir la magnitud máxima de un vector.

**Actividad 6**
1. 

**Actividad 7**
1. Motion 101 es el modelo vectorial mínimo para simular movimiento: la aceleración modifica la velocidad y la velocidad modifica la posición.
2. El Motion 101 se encuentra en la línea this.position.add(this.velocity); donde la posición cambia según la velocidad constante.

**Actividad 8**
1. Con la aceleración constante, la velocidad aumenta cxada frame, con una dirección fija, y se mueve cada vez más rápido. En la aceleración aleatoria, la dirección cambia constantemente, el movimiento es impredecible y la trayectoria es como temblorosa o errática. En la aceleración hacia el mouse, el objeto persigue el mouse, la trayectoria es curva, si la velocidad no es limitada puede ser muy rápido.

## Bitácora de aplicación 

1. Decidí hacer una especie de versión mejorada de mi obra anterior, quería mejorar ese "lag" que geneeraban tantas partículas, y que tuviera una apariencia mucho más estética. Combinando colores, logré que se pareciera un poco a una galaxia, para mí. Mi obra anterior siento que fue una exploración en lo desconocido, y aquí si quise que tuviera algo más de sentido y se viera bien. Las partículas esta vez tienen un movimiento más fluido y dinámico, que aunque es aleatorio, se sigue viendo armonioso en conjunto.


```js
let particles = [];

function setup() {
  createCanvas(800, 500);
  colorMode(HSB, 360, 100, 100, 100);
  
  for (let i = 0; i < 80; i++) {
    particles.push(new Particle());
  }
  
  background(0);
}

function draw() {
  fill(0, 0, 0, 10); 
  noStroke();
  rect(0, 0, width, height);

  for (let p of particles) {
    p.update();
    p.edges();
    p.show();
  }
}

class Particle {
  constructor() {
    this.position = createVector(random(width), random(height));
    this.velocity = p5.Vector.random2D();
    this.velocity.setMag(random(1, 2));
    this.acceleration = createVector(0, 0);
  }

  update() {

 
    let randomAcc = p5.Vector.random2D();
    randomAcc.setMag(0.1);

    let mouse = createVector(mouseX, mouseY);
    let attraction = p5.Vector.sub(mouse, this.position);
    attraction.normalize();
    attraction.mult(0.2);

    this.acceleration = createVector(0, 0);
    this.acceleration.add(randomAcc);
    this.acceleration.add(attraction);


    this.velocity.add(this.acceleration);
    this.velocity.limit(4);
    this.position.add(this.velocity);
  }

  edges() {
    if (this.position.x > width) this.position.x = 0;
    if (this.position.x < 0) this.position.x = width;
    if (this.position.y > height) this.position.y = 0;
    if (this.position.y < 0) this.position.y = height;
  }

  show() {
    let speed = this.velocity.mag();
    let hue = map(speed, 0, 4, 180, 360);

    noStroke();
    fill(hue, 80, 100, 80);
    circle(this.position.x, this.position.y, 8);
  }
}
```

[Enlace al proyecto](https://editor.p5js.org/CatLikeKenma/sketches/f7-BMTHYF)

<img width="299" height="305" alt="Captura de pantalla 2026-02-11 222921" src="https://github.com/user-attachments/assets/9fb0bd18-9963-4b35-8753-e6657bb9a976" />

## Bitácora de reflexión

1. Experimentando con la IA, me di cuenta que era posible hacer unas conexiones entre partículas, conexiones visibles. Esto me recordó a las neuronas, las conexiones neurológicas que se forman al crear recuerdos, aprender cosas nuevas, etc. Entonces quise hacer algo que recordara a ese tipo de conexiones.
2. 
```js
let nodes = [];
let attract = true;

function setup() {
  createCanvas(800, 600);
  background(10);

  for (let i = 0; i < 90; i++) {
    nodes.push(new Node(random(width), random(height)));
  }
}

function draw() {
  fill(10, 40);
  noStroke();
  rect(0, 0, width, height);

  for (let n of nodes) {
    n.reactToMouse();
    n.update();
    n.display();
  }

  drawConnections();
}

class Node {
  constructor(x, y) {
    this.position = createVector(x, y);
    this.velocity = createVector(random(-1,1), random(-1,1));
    this.acceleration = createVector(0, 0);
    this.maxSpeed = 3;
  }

  reactToMouse() {
    let mouse = createVector(mouseX, mouseY);
    let force = p5.Vector.sub(mouse, this.position);

    let distance = force.mag();

    if (distance < 200) {
      force.normalize();

      if (!attract) {
        force.mult(-1); // repulsión
      }

      force.mult(0.3);
      this.acceleration = force;
    }
  }

  update() {
    this.velocity.add(this.acceleration);
    this.velocity.limit(this.maxSpeed);
    this.position.add(this.velocity);

    this.acceleration.mult(0);


    if (this.position.x > width || this.position.x < 0) {
      this.velocity.x *= -1;
    }

    if (this.position.y > height || this.position.y < 0) {
      this.velocity.y *= -1;
    }
  }

  display() {

    let size = this.velocity.mag() * 3;

    noStroke();
    fill(255);
    circle(this.position.x, this.position.y, size);
  }
}

function drawConnections() {
  for (let i = 0; i < nodes.length; i++) {
    for (let j = i + 1; j < nodes.length; j++) {

      let diff = p5.Vector.sub(nodes[i].position, nodes[j].position);
      let d = diff.mag();

      if (d < 70) {
        let alpha = 150 - d * 2;
        stroke(255, alpha);
        line(
          nodes[i].position.x,
          nodes[i].position.y,
          nodes[j].position.x,
          nodes[j].position.y
        );
      }
    }
  }
}

function mousePressed() {
  attract = !attract; 
}
```
3. [Enlace](https://editor.p5js.org/CatLikeKenma/sketches/G0_sRIqYo)
4. <img width="679" height="521" alt="Captura de pantalla 2026-02-13 213115" src="https://github.com/user-attachments/assets/e117cc6d-ef60-40ff-803b-86a15e33577c" />




