# Unidad 1

## 🔎 Fase: Set + Seek

## ¿Qué es un sistema físico interactivo?
Un sistema físico interactivo es un conjunto de componentes capaz de generar obras con cierto grado de aleatoriedad, o que responda a estimulos externos.

## ¿Qué es el diseño y el arte generativos?
 Es mediante sistemas, usualmente codigos usando tecnologías tales como plotters, para hacer obras con cierto grado de aletoriedad, y al mismo tiempo automarizar el proceso mientras se mantiene unas reglas

## como usarlo en mi perfil profesional? 
 Podría aprender de los algoritmos que usan para agregar un componente de aletoriedad manteniendo ciertas reglas, lo cual siento que puede ser util para juegos estilo rogue-like ( que los escenarios se van creando proceduralmente/aletoriamente, con ciertas reglas ). Y en tema de diseño de cinterfaces puede dar ideas interesantes para diferentes necsidades, O incluso para elementos que respondan con la interacción del usuario 


## Actividad 3, inputs, outpus, proceso

Del computador chiquito El cable usb puede ser tanto input como output, dependiendo de si estamos usando " send love ", o si estamos presionando un boton o usando el acelerometro. Boton y acelerometro también funcionan como inputs, el display es un output

Del pc, el input tambien es el cable usb y el boton de send love( serial ), los output pueden ser tanto la pantalla como el usb

[Mi código](https://editor.p5js.org/Juandavid1612/sketches/pIJ7XDj5n)

 ``` let balls = [];
let speedSlider, ballInput, applyButton;

function setup() {
  createCanvas(600, 400);
  angleMode(RADIANS);

  // velocidad
  speedSlider = createSlider(0.01, 0.2, 0.05, 0.01);
  speedSlider.position(10, height + 10);
  speedSlider.style('width', '120px');

  //  cantidad de bolas
  ballInput = createInput('5', 'number');
  ballInput.position(150, height + 10);
  ballInput.size(50);

  applyButton = createButton('Aplicar');
  applyButton.position(210, height + 10);
  applyButton.mousePressed(resetBalls);

  resetBalls();
}

function draw() {
  background(30);
  let speed = speedSlider.value();

  for (let ball of balls) {
    ball.update(speed);
    ball.display();
  }


  fill(255);
  noStroke();
  text('Velocidad', 10, height + 40);
  text('Cantidad', 150, height + 40);
}

function resetBalls() {
  let count = int(ballInput.value());
  balls = [];

  for (let i = 0; i < count; i++) {
    balls.push(new OscillatingBall(random(width), random(height), random(20, 40)));
  }
}

class OscillatingBall {
  constructor(x, y, radius) {
    this.x0 = x;
    this.y0 = y;
    this.r = radius;
    this.angle = random(TWO_PI);
    this.radiusOffset = random(30, 100);
  }

  update(speed) {
    this.angle += speed;
  }

  display() {
    let x = this.x0 + cos(this.angle) * this.radiusOffset;
    let y = this.y0 + sin(this.angle) * this.radiusOffset;
    fill(100, 200, 255);
    noStroke();
    ellipse(x, y, this.r);
  }
}
```


## Inputs:
* Boton para cambiar la cantidad de bolas
* Slide para cambiar la velocidad

## Outputs:
* Pantalla


https://github.com/user-attachments/assets/db06497d-0a3a-40ac-9929-db1baf866a4b




