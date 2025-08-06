# Unidad 1


## 🛠 Fase: Apply

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
