
# Evidencias de la unidad 4

## Código

[Enlace a la aplicación a modificar](http://www.generative-gestaltung.de/2/sketches/?01_P/P_1_0_01)

Código a modificar:

``` 
// P_1_0_01
//
// Generative Gestaltung – Creative Coding im Web
// ISBN: 978-3-87439-902-9, First Edition, Hermann Schmidt, Mainz, 2018
// Benedikt Groß, Hartmut Bohnacker, Julia Laub, Claudius Lazzeroni
// with contributions by Joey Lee and Niels Poldervaart
// Copyright 2018
//
// http://www.generative-gestaltung.de
//
// Licensed under the Apache License, Version 2.0 (the "License");
// you may not use this file except in compliance with the License.
// You may obtain a copy of the License at http://www.apache.org/licenses/LICENSE-2.0
// Unless required by applicable law or agreed to in writing, software
// distributed under the License is distributed on an "AS IS" BASIS,
// WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
// See the License for the specific language governing permissions and
// limitations under the License.

/**
 * changing colors and size by moving the mouse
 *
 * MOUSE
 * position x          : size
 * position y          : color
 *
 * KEYS
 * s                   : save png
 */
'use strict';

function setup() {
  createCanvas(720, 720);
  noCursor();

  colorMode(HSB, 360, 100, 100);
  rectMode(CENTER);
  noStroke();
}

function draw() {
  background(mouseY / 2, 100, 100);

  fill(360 - mouseY / 2, 100, 100);
  rect(360, 360, mouseX + 1, mouseX + 1);
}

function keyPressed() {
  if (key == 's' || key == 'S') saveCanvas(gd.timestamp(), 'png');
}

```

[Enlace a la aplicación modificada](https://editor.p5js.org/Juandavid1612/full/AiTt468B3)

Código modificado:

``` 
function updateButtonStates(newAState, newBState) {
  // Botón A: limpiar pantalla
  if (newAState === true && prevmicroBitAState === false) {
    background(255);
    print("A pressed: Canvas cleared!");
  }

  // Botón B: cambiar color aleatorio
  if (newBState === true && prevmicroBitBState === false) {
    c = color(random(255), random(255), random(255));
    print("B pressed: Color changed!");
  }

  prevmicroBitAState = newAState;
  prevmicroBitBState = newBState;
}

function draw() {
  if (!port.opened()) {
    connectBtn.html("Connect to micro:bit");
    microBitConnected = false;
  } else {
    microBitConnected = true;
    connectBtn.html("Disconnect");
  }

  // Lectura de datos del micro:bit
  if (port.availableBytes() > 0) {
    let data = port.readUntil("\n");
    if (data) {
      data = data.trim();
      let values = data.split(",");
      if (values.length == 4) {
        microBitX = int(values[0]);
        microBitY = int(values[1]);
        microBitAState = values[2].toLowerCase() === "true";
        microBitBState = values[3].toLowerCase() === "true";
        updateButtonStates(microBitAState, microBitBState);
      } else {
        print("No se están recibiendo 4 datos del micro:bit");
      }
    }
  }

  switch (appState) {
    case STATES.WAIT_MICROBIT_CONNECTION:
      if (microBitConnected === true) {
        print("Microbit ready to draw");
        strokeWeight(0.75);
        c = color(0);
        noCursor();
        appState = STATES.RUNNING;
      }
      break;

    case STATES.RUNNING:
      // Mapeo del microBitX (acelerómetro en X) a tamaño de letra
      fontSize = map(microBitX, -1024, 1024, fontSizeMin, fontSizeMax);
      fontSize = constrain(fontSize, fontSizeMin, fontSizeMax);

      // Mapeo del microBitY (acelerómetro en Y) a rotación
      let angle = map(microBitY, -1024, 1024, -PI / 2, PI / 2);

      textSize(fontSize);
      let newLetter = letters.charAt(counter);
      stepSize = textWidth(newLetter);

      push();
      translate(x, y);
      rotate(angle);
      fill(c);
      text(newLetter, 0, 0);
      pop();

      counter++;
      if (counter >= letters.length) counter = 0;

      // Avanza posición automáticamente en diagonal
      x += stepSize * 0.5;
      y += stepSize * 0.5;

      // Reinicio de posición si se sale del canvas
      if (x > width || y > height) {
        x = 50;
        y = 50;
      }
      break;
  }
}

```

## Video

[Video demostratativo](https://youtu.be/nie_bIS7t1U)











