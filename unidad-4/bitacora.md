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

  'use strict';

let port;
let reader;
let xValue = 0;
let yValue = 0;
let aState = 0;
let bState = 0;
let connectBtn;

let angle = 0; // rotación del rectángulo

function setup() {
  createCanvas(720, 720);
  noCursor();

  colorMode(HSB, 360, 100, 100);
  rectMode(CENTER);
  noStroke();

  connectBtn = createButton("Conectar Micro:bit");
  connectBtn.position(20, 20);
  connectBtn.mousePressed(connect);
}

function draw() {
  background(map(yValue, -1024, 1024, 0, 180), 100, 100);
  fill(360 - map(yValue, -1024, 1024, 0, 180), 100, 100);

  // Rotación con A (derecha) y B (izquierda)
  if (aState === 1) angle += 2;
  if (bState === 1) angle -= 2;

  push();
  translate(width / 2, height / 2);
  rotate(radians(angle));
  const s = map(xValue, -1024, 1024, 10, width);
  rect(0, 0, s, s);
  pop();

  // (Opcional) HUD para depurar
  noStroke();
  fill(0, 0, 20);
  rect(10, height - 40, 260, 30);
  fill(0);
  text(`x:${xValue}  y:${yValue}  A:${aState}  B:${bState}`, 20, height - 20);
}

// ===== Conexión Web Serial =====
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
  const decoder = new TextDecoder();

  while (true) {
    const { value, done } = await reader.read();
    if (done) {
      reader.releaseLock();
      break;
    }
    if (!value) continue;

    buffer += decoder.decode(value);
    // Soporta \n y \r\n
    let lines = buffer.split(/\r?\n/);
    buffer = lines.pop(); // última línea (posible incompleta)

    for (let line of lines) {
      line = line.trim();
      if (!line) continue;
      const parts = line.split(",");
      if (parts.length !== 4) continue;

      // Parseo robusto
      const xi = parseInt(parts[0], 10);
      const yi = parseInt(parts[1], 10);
      if (!Number.isNaN(xi)) xValue = xi;
      if (!Number.isNaN(yi)) yValue = yi;

      // Acepta "1"/"0" o "True"/"False" (cualquier may/min)
      const to01 = (v) => {
        const t = String(v).trim().toLowerCase();
        if (t === '1' || t === 'true') return 1;
        return 0;
      };
      aState = to01(parts[2]);
      bState = to01(parts[3]);
    }
  }
}


```

## Video

[Video demostratativo](https://youtu.be/nie_bIS7t1U)










