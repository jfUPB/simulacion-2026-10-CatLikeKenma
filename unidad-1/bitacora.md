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
1. function setup() {
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
![Uploading Captura de pantalla 2026-01-28 164143.png…]()

## Bitácora de aplicación 



## Bitácora de reflexión

