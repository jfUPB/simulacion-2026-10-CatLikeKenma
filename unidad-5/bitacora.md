# Unidad 5
## Bitácora de proceso de aprendizaje
**Actividad 01**
1. Tienen Motion 101, con velocidad. posición y aceleración, como estado físico. Y un lifespan que indica cuando muere la partícula.
2. Muere de forma gradual, no instantánea.
3. Con el patrón de Motion 101, la aceleración cambia velocidad, la velocidad cambia posición, y al final se limpia la aceleración.
4. Las crea la función draw(), y se crean cada frame
5. La función draw las elimina en la linea "if particle is dead"
6. Porque se están eliminando elementos
7. El array crece infinitamente y los fotogramas bajan, se vuelve lento y podría crashear.
8. Con una posición, una forma y un color.
9. A menor lifespan más transparente hasta que desaparece y así se ve el ciclo de vida.
10. Se cambia el display

**Actividad 02**
1. 
## Bitácora de aplicación 
1. "Memorias que Flotan" Representaré la nostalgia con un concepto acuático, porque es de mis conceptos y estéticas favoritas. Se me ocurrió representar las memorias en forma de burbujas, me inspiré un poco de la película Intensamente. Nostalgia es un personaje que me recuerda a mi abuela, y ella siempre me cuenta historias, entonces pense en el nacimiento de una burbuja como el evento ocurriendo, o la creación del recuerdo, después crece y se mantiene, hasta que se olvida PERO no desaparece completamente, porque la nostalgia es el arte de recordar solo las flores y olvidar las espinas.
2. https://editor.p5js.org/CatLikeKenma/sketches/4YUph8DqS
3. let particles = [];

function setup() {
  createCanvas(640,480);
  colorMode(HSB,255);
}

function draw() {


  for(let y = 0; y < height; y++){
    let inter = map(y, 0, height, 0, 1);

    let c1 = color(170, 180, 180); // agua clara
    let c2 = color(200, 200, 40);  // profundidad

    let c = lerpColor(c1, c2, inter);

    stroke(c);
    line(0, y, width, y);
  }


  noStroke();
  fill(170,50,100,20);
  rect(0,0,width,height);

  translate(width/2,height/2);

  for(let i = particles.length-1; i >= 0; i--){
    let p = particles[i];
    p.update();
    p.display();

    if(p.isDead()){
      particles.splice(i,1);
    }
  }
}


class Particle{
  constructor(x,y){
    this.pos = createVector(x,y);
    this.vel = createVector(0,0);
    this.acc = createVector(0,0);
    this.life = 255;
  }

  applyForce(f){
    this.acc.add(f);
  }

  update(){
    this.vel.add(this.acc);
    this.pos.add(this.vel);
    this.acc.mult(0);
    this.life -= 2;
  }

  isDead(){
    return this.life <= 0;
  }
}


class Bubble extends Particle{
  constructor(x,y){
    super(x,y);
this.r = random(40,120);
  }

  update(){


    let buoyancy = createVector(0,-0.03);
    this.applyForce(buoyancy);

    let mouse = createVector(mouseX-width/2, mouseY-height/2);
    let flow = p5.Vector.sub(mouse,this.pos);
    flow.setMag(0.02);
    this.applyForce(flow);

    super.update();

    // crece ligeramente
this.r += 0.3;


    if(this.life < 80){
      for(let i=0;i<6;i++){
        particles.push(new Foam(this.pos.x,this.pos.y));
      }
      this.life = 0;
    }
  }

  display(){

    push();
    translate(this.pos.x,this.pos.y);

    noFill();

    // borde
    stroke(180,80,255,this.life);
    strokeWeight(1);
    ellipse(0,0,this.r);

    // brillo
    noStroke();
    fill(180,50,255,80);
    ellipse(-this.r*0.3,-this.r*0.3,this.r*0.3);

    pop();
  }
}


class Foam extends Particle{
  constructor(x,y){
    super(x,y);
    this.vel = p5.Vector.random2D().mult(random(0.5,2));
  }

  update(){

    // flotación suave
    let up = createVector(0,-0.01);
    this.applyForce(up);

    super.update();
  }

  display(){

    push();
    translate(this.pos.x,this.pos.y);

    noStroke();

    // glow
    for(let i=2;i>0;i--){
      fill(180,50,255,20);
      ellipse(0,0,i*3);
    }

    fill(180,80,255,this.life);
    ellipse(0,0,2);

    pop();
  }
}


function mousePressed(){
  let x = mouseX - width/2;
  let y = mouseY - height/2;

  particles.push(new Bubble(x,y));
}

4.<img width="152" height="180" alt="Captura de pantalla 2026-03-25 221522" src="https://github.com/user-attachments/assets/66e9dc66-000e-427b-a315-cc3ed4608f3d" />
<img width="161" height="136" alt="Captura de pantalla 2026-03-25 221530" src="https://github.com/user-attachments/assets/344cce5f-cf6a-4809-a964-6776444bd5b7" />
<img width="223" height="162" alt="Captura de pantalla 2026-03-25 221540" src="https://github.com/user-attachments/assets/2dd7c848-f208-4696-bc22-fff91bfb8dec" />

## Bitácora de reflexión
