# Evidencias de la unidad 4

## Código

### SE HA CAMBIADO LA OBRA DE ARTE GENERATIVO

[Enlace a la aplicación a modificar](https://editor.p5js.org/generative-design/sketches/P_2_3_3_01)

Código a modificar:

``` js
// M_2_3_01
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
 * draws an amplitude modulated oscillator
 *
 * KEYS
 * i                 : toggle draw info signal
 * c                 : toggle draw carrier signal
 * 1/2               : info signal frequency -/+
 * arrow left/right  : info signal phi -/+
 * 7/8               : carrier signal frequency -/+ (modulation frequency)
 * s                 : save png
 */
'use strict';

var sketch = function(p) {

  var pointCount = 600;
  var freq = 2;
  var phi = 0;
  var modFreq = 12;

  var drawFrequency = true;
  var drawModulation = true;
  var drawCombination = true;

  var angle;
  var y;

  p.setup = function() {
    p.createCanvas(p.windowWidth,800);
    p.noFill();
    pointCount = p.width;
  };

  p.draw = function() {
    p.background(255);
    p.strokeWeight(1);

    p.translate(0, p.height / 2);

    // draw oscillator with freq and phi
    if (drawFrequency) {
      p.beginShape();
      for (var i = 0; i <= pointCount; i++) {
        angle = p.map(i, 0, pointCount, 0, p.TAU);
        y = p.sin(angle * freq + p.radians(phi));
        y *= p.height / 4;
        p.vertex(i,y);
      }
      p.endShape();
    }

    // draw oscillator with modFreq
    if (drawModulation) {
      p.stroke(0,130,164,128);
      p.beginShape();
      for (var i = 0; i <= pointCount; i++) {
        angle = p.map(i, 0, pointCount, 0, p.TAU);
        y = p.cos(angle * modFreq);
        y *= p.height / 4;
        p.vertex(i,y);
      }
      p.endShape();
    }

    // draw both combined
    p.stroke(0);
    p.strokeWeight(2);
    p.beginShape();
    for (var i = 0; i <= pointCount; i++) {
      angle = p.map(i, 0, pointCount, 0, p.TAU);
      var info = p.sin(angle * freq + p.radians(phi));
      var carrier = p.cos(angle * modFreq);
      y = info * carrier;
      y *= p.height / 4;
      p.vertex(i,y);
    }
    p.endShape();
  };

  p.keyPressed = function() {
    if (p.key == 's' || p.key == 'S') p.saveCanvas(gd.timestamp(), 'png');

    if (p.key == 'i' || p.key == 'I') drawFrequency = !drawFrequency;
    if (p.key == 'c' || p.key == 'C') drawModulation = !drawModulation;

    if (p.key == '1') freq--;
    if (p.key == '2') freq++;
    freq = p.max(freq,1);

    if (p.keyCode == p.LEFT_ARROW) phi -= 15;
    if (p.keyCode == p.RIGHT_ARROW) phi += 15;

    if (p.key == '7') modFreq--;
    if (p.key == '8') modFreq++;
    modFreq = p.max(modFreq,1);

    console.log('freq: ' + freq + ', phi: ' + phi + ', modFreq: ' + modFreq);
  };

};

var myp5 = new p5(sketch);

```

[Enlace a la aplicación modificada]([https://editor.p5js.org/cookielolwow/sketches/PAx7VYK9Y](https://editor.p5js.org/Juandavid1612/full/chDIdqana))

Código modificado:

``` js
var x = 0;
var y = 0;
var stepSize = 5.0;

let fontSize = 10; 
let fontSizeMax = 72;
let fontSizeMin = 3;
var font = 'Georgia';
var letters = 'All the world\'s a stage, and all the men and women merely players. They have their exits and their entrances.';

var angleDistortion = 0.0;

var counter = 0;
let microBitX = 0;
let microBitY = 0;
let microBitAState = false;
let microBitBState = false;
let prevmicroBitAState = false;
let prevmicroBitBState = false;
let c;

//*********************************
// Código para soportar el serial
let port;
let connectBtn;
let microBitConnected = false;
//*********************************

const STATES = {
  WAIT_MICROBIT_CONNECTION: "WAITMICROBIT_CONNECTION",
  RUNNING: "RUNNING",
};

let appState = STATES.WAIT_MICROBIT_CONNECTION;

function setup() {
  createCanvas(displayWidth, displayHeight);
  background(255);
  cursor(CROSS);

  x = mouseX;
  y = mouseY;

  textFont(font);
  textAlign(LEFT);
  fill(0);
  c = color(1, 1, 1, 1);
  port = createSerial();
  connectBtn = createButton("Connect to micro:bit");
  connectBtn.position(0, 0);
  connectBtn.mousePressed(connectBtnClick);
}

function connectBtnClick() {
  if (!port.opened()) {
    port.open("MicroPython", 115200);
    connectionInitialized = false;
  } else {
    port.close();
  }
}

function updateButtonStates(newAState, newBState) {
  if (newAState === true && prevmicroBitAState === false) {
    x = microBitX;
    y = microBitY;
    print("A pressed: start drawing at", x, y);
  }

   if (newBState === true && prevmicroBitBState === false) {
    // Aquí guardamos la captura
    saveCanvas('microbit_screenshot_' + Date.now(), 'png');
    print("B pressed: screenshot taken!");
  }

  prevmicroBitAState = newAState;
  prevmicroBitBState = newBState;
}


function draw() {
  
  
  if (!port.opened()) 
  {
    connectBtn.html("Connect to micro:bit");
    microBitConnected = false;
  } else {
    microBitConnected = true;
    connectBtn.html("Disconnect");
  }

      if (port.availableBytes() > 0) {
      let data = port.readUntil("\n");
      if (data) {
        data = data.trim();
        let values = data.split(",");
        if (values.length == 4) {
          microBitX = int(values[0]) + windowWidth / 2;
          microBitY = int(values[1]) + windowHeight / 2;
          microBitAState = values[2].toLowerCase() === "true";
          microBitBState = values[3].toLowerCase() === "true";
          updateButtonStates(microBitAState, microBitBState);
        } else {
          print("No se están recibiendo 4 datos del micro:bit");
        }
      }
    }
  
  
  switch (appState) 
  {
 case STATES.WAIT_MICROBIT_CONNECTION:
      if (microBitConnected === true) {
        // Preparo todo para el estado en el próximo frame
        print("Microbit ready to draw");
        strokeWeight(0.75);
        c = color(1, 1, 1);
        noCursor();
        appState = STATES.RUNNING;
      }

      break;
     

    case STATES.RUNNING:
            print(`Estado RUNNING: A:${microBitAState}, B:${microBitBState}, X:${microBitX}, Y:${microBitY}, Pos dibujo: (${x},${y}), FontSize: ${fontSize}`);
      if (microBitAState === true) {
  let d = dist(x, y, microBitX, microBitY);
  
 
  let targetFontSize = map(d, 0, 50, fontSizeMin, fontSizeMax);
  
  
  fontSize = lerp(fontSize, targetFontSize, 0.1);
  
  textSize(fontSize);
  let newLetter = letters.charAt(counter);
  stepSize = textWidth(newLetter);

  if (d > stepSize) {
    var angle = atan2(microBitY - y, microBitX - x);

    push();
    translate(x, y);
    rotate(angle + random(angleDistortion));
    fill(c);
    text(newLetter, 0, 0);
    pop();

    counter++;
    if (counter >= letters.length) counter = 0;

    x = x + cos(angle) * stepSize;
    y = y + sin(angle) * stepSize;
  }


}

      break;
  }
  
}

function mousePressed() {
  x = mouseX;
  y = mouseY;
}

function keyReleased() {
  if (key == 's' || key == 'S') saveCanvas(gd.timestamp(), 'png');
  if (keyCode == DELETE || keyCode == BACKSPACE) background(255);
}

function keyPressed() {
  // angleDistortion ctrls arrowkeys up/down
  if (keyCode == UP_ARROW) angleDistortion += 0.1;
  if (keyCode == DOWN_ARROW) angleDistortion -= 0.1;
}

```

















