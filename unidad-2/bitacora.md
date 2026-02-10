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

## Bitácora de aplicación 


## Bitácora de reflexión


