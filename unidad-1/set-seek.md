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

## Act 5 

### micro;bit

En un bucle infinito (while True):

Si el botón A del micro:bit está presionado, envía la letra 'A' por el puerto UART.

Si no está presionado, envía 'N'.

Espera 100 milisegundos antes de volver a verificar.



### p5.js

Dibuja un lienzo de 400x400 píxeles.

Crea un botón para conectarse al micro:bit.

Cuando recibe un dato por serial:

Si es "A", pinta un cuadro rojo.

Si es "N", pinta un cuadro verde.

El botón cambia entre “Connect” y “Disconnect” dependiendo del estado de la conexión.


## Act 6

### Python
```
from microbit import *

import utime
uart.init(baudrate=115200)

while True:
    if button_a.is_pressed():
        uart.write('L')
    elif button_b.is_pressed():
        uart.write('R')
    else:
        uart.write('N')

    sleep(100)

```
### javascript
```
let port;
let connectBtn;
let connectionInitialized = false;

let circleX;

function setup() {
  createCanvas(400, 400);
  circleX = width / 2;

  port = createSerial();
  connectBtn = createButton("Connect to micro:bit");
  connectBtn.position(20, 20);
  connectBtn.mousePressed(connectBtnClick);
}

function draw() {
  background(220);

  if (port.opened() && !connectionInitialized) {
    port.clear();
    connectionInitialized = true;
  }

  if (port.availableBytes() > 0) {
    let data = port.read(1);
    if (data === "L") {
      circleX -= 5; // Mover a la izquierda
    } else if (data === "R") {
      circleX += 5; // Mover a la derecha
    }
  }

  circleX = constrain(circleX, 0, width);

  // círculo
  fill("blue");
  circle(circleX, height / 2, 50);

  // Cambiar texto 
  if (!port.opened()) {
    connectBtn.html("Connect to micro:bit");
  } else {
    connectBtn.html("Disconnect");
  }
}

function connectBtnClick() {
  if (!port.opened()) {
    port.open("MicroPython", 115200);
    connectionInitialized = false;
  } else {
    port.close();
  }
}

```
## Autoevaluación

### parte 1 

1. Siempre recibe información del entorno a traves de sensores/controladores ( INPUT), El sistema analiza o interpreta los datos de entrada ( PROCESO ), El sistema responde de manera perceptible sea de forma digital ( por pantalla ) o física ( OUTPUT) 

2. El input fueron la presión de los botones A y B del microbit, el proceso es enviar un caracter dependiendo de qué boton se presione y el p5.js decide si moverlo a izquierda o derecha, el Output es el movimiento del circulo en pantalla a la direccion corerspondiente

3. uart.write("A") envía el mensaje por serial, el port.read(1) "escucha" el mensaje

4. El arte tradicional es creado manualmente por el artista mientras que el arte generativo usa algoritmoso reglas para generar obras de forma semi automatica

5. Programar el micro:bit para que detecte cuando se sacude usando su acelerómetro, cuando se detecte la sacudida, enviar un mensaje (como una letra) a través del puerto serial hacia la computadora. Programar el sistema para que escuche los datos que llegan por el puerto serial y cuando reciba el mensaje de “sacudida”, cambiar el color del círculo en pantalla a un color aleatorio.

### parte 2

1. La parte tecnica, porque hace mucho no me tomaba el tiempo de entender un código y honestamente estoy ya un poco saturado de la programación en general

2. No sabía como hacer el ejercicio 4, fue cuando revisé la pagina de p5.js que entendí mejor el lenguaje de programación y pude sacarlo adelante

3. Este curso puede servir para llevar más a fondo el entedimiento de lo que programemos, hacer piezas creativas a base del mismo, etc

4. Me gustó ese aprendizaje guiado

## Coevaluación

El código de mi compañero Alejandro Gonzales estaba claro, lo comentó bien y de forma clara, vi un buen uso de las funciones para ejecutar su idea en p5.js

## Feedback

1. El video de esa chica que hacía cuadros con codigo me ayudó mucho a entenderlo

2. La vdd no tengo un comentario en esta parte

3. No, no me gustaría explorar más al respecto

4. Pues, entre frustrante e interesante, frustrante por no entender cómo hacer cosas de ese estilo e interesado por los resultados

5. No.


# Unidad 2

   
## Act 1

1.

La clase Pixel define un pixel con coordenadas y un intervalode tiempo, que determina cada cuanto cambia el estado entre encendido y apagado, en Init se guarda el estado inicial, el tiempo actual, se enciende o apaga el pixel y luego cambia a WaitTimeout, revisa si ha pasado el intervalo de tiempo definido, en caso tal se cambia el estado de pixel y actualiza el tiempo de inicio para contar otro intervalo


2.

Init y WaitTimeout

3.

Este programa no usa eventos externos, unicamente el paso del tiempo

4.

Encender o apagar un pixel, cambiar el estado del pixel, actualizar el tiempo


## Act 2

Estados: Rojo, Verde y Amarillo

Eventos: Paso del tiempo

Acciones: Encender solo el Led correspondiente, esperar a que pase el tiempo, actualizar el tiempo de referencia

```
from microbit import *
import utime


LED_ROJO = (0, 2)
LED_AMARILLO = (2, 2)
LED_VERDE = (4, 2)

class Semaforo:
    def __init__(self):
        self.estado = "Rojo"
        self.tiempo_inicio = utime.ticks_ms()
        self.intervalo = 3000  # 3 segundos por estado

    def encender_led(self, color):
     
        display.clear()
        if color == "Rojo":
            display.set_pixel(LED_ROJO[0], LED_ROJO[1], 9)
        elif color == "Amarillo":
            display.set_pixel(LED_AMARILLO[0], LED_AMARILLO[1], 9)
        elif color == "Verde":
            display.set_pixel(LED_VERDE[0], LED_VERDE[1], 9)

    def actualizar(self):
        tiempo_actual = utime.ticks_ms()
        if utime.ticks_diff(tiempo_actual, self.tiempo_inicio) > self.intervalo:
            self.tiempo_inicio = tiempo_actual  # Reiniciar tiempo

          
            if self.estado == "Rojo":
                self.estado = "Verde"
            elif self.estado == "Verde":
                self.estado = "Amarillo"
            elif self.estado == "Amarillo":
                self.estado = "Rojo"

        self.encender_led(self.estado)


semaforo = Semaforo()


while True:
    semaforo.actualizar()
    sleep(100)

```

