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

[Enlace a la aplicación modificada]()

Código modificado:

``` 

  'use strict';

let port;
let reader;
let xValue = 0;
let yValue = 0;
let aState = 0;
let bState = 0;
let connectBtn;

function setup() {
  createCanvas(720, 720);
  noCursor();

  colorMode(HSB, 360, 100, 100);
  rectMode(CENTER);
  noStroke();

  // Botón para conectar el micro:bit
  connectBtn = createButton("Conectar Micro:bit");
  connectBtn.position(20, 20);
  connectBtn.mousePressed(connect);
}

function draw() {
  background(map(yValue, -1024, 1024, 0, 180), 100, 100);

  fill(360 - map(yValue, -1024, 1024, 0, 180), 100, 100);
  rect(360, 360, map(xValue, -1024, 1024, 10, width), map(xValue, -1024, 1024, 10, width));

  // Botón A guarda la imagen
  if (aState === 1) {
    saveCanvas('microbit_rect', 'png');
  }

  // Botón B limpia el fondo en blanco
  if (bState === 1) {
    background(0, 0, 100);
  }
}

// =========================
// 🔗 Conexión con micro:bit
// =========================
async function connect() {
  try {
    port = await navigator.serial.requestPort();
    await port.open({ baudRate: 115200 });

    reader = port.readable.getReader();
    readLoop();
  } catch (err) {
    console.error("Error al conectar:", err);
  }
}

async function readLoop() {
  let buffer = "";
  while (true) {
    const { value, done } = await reader.read();
    if (done) {
      reader.releaseLock();
      break;
    }
    if (value) {
      buffer += new TextDecoder().decode(value);
      let lines = buffer.split("\n");
      buffer = lines.pop();

      for (let line of lines) {
        let parts = line.trim().split(",");
        if (parts.length === 4) {
          xValue = int(parts[0]);
          yValue = int(parts[1]);
          aState = int(parts[2]);
          bState = int(parts[3]);
        }
      }
    }
  }
}


```

## Video

[Video demostratativo](URL)







