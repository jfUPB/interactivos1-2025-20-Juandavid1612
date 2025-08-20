# Unidad 3


## 🛠 Fase: Apply


## Act 6

```

let port;
let writer;
let reader;

const CONFIG = "CONFIG";
const ARMED = "ARMED";
const EXPLODED = "EXPLODED";

let state = CONFIG;
let count = 20;
let startTime = 0;
let password = ['A', 'B', 'A'];
let key = [];
let keyIndex = 0;
let serialBuffer = "";

function setup() {
  createCanvas(400, 300);
  textAlign(CENTER, CENTER);
  textSize(24);
  startTime = millis();

  let connectBtn = createButton("Conectar Serial");
  connectBtn.position(10, 10);
  connectBtn.mousePressed(connectSerial);
}

function draw() {
  background(30);
  fill(255);
  text("Estado: " + state, width / 2, 50);
  text("Tiempo: " + count, width / 2, 100);

  if (state === CONFIG) {
    text("Aumentar: 'A'  Disminuir: 'B'  Armar: 'S'", width / 2, 200);
  } else if (state === ARMED) {
    text("Clave: A-B-A", width / 2, 200);
    if (millis() - startTime > 1000) {
      startTime = millis();
      count--;
      if (count <= 0) {
        state = EXPLODED;
      }
    }
  } else if (state === EXPLODED) {
    fill(255, 0, 0);
    text("💀 BOOM 💀", width / 2, height / 2);
    text("Reiniciar: 'R'", width / 2, 200);
  }

  if (reader) {
    readSerial();
  }
}

function keyPressed() {
  handleEvent(key.toUpperCase());
}

function handleEvent(ev) {
  if (state === CONFIG) {
    if (ev === 'A') {
      count = min(count + 1, 60);
    } else if (ev === 'B') {
      count = max(count - 1, 10);
    } else if (ev === 'SHAKE' || ev === 'S') {
      state = ARMED;
      startTime = millis();
    }
  } else if (state === ARMED) {
    if (ev === 'A' || ev === 'B') {
      key[keyIndex] = ev;
      keyIndex++;
      if (keyIndex >= password.length) {
        if (arraysEqual(key, password)) {
          count = 20;
          keyIndex = 0;
          key = [];
          state = CONFIG;
        } else {
          keyIndex = 0;
          key = [];
        }
      }
    }
  } else if (state === EXPLODED) {
    if (ev === 'RESET' || ev === 'R') {
      count = 20;
      state = CONFIG;
      keyIndex = 0;
      key = [];
    }
  }
}

function arraysEqual(a, b) {
  if (a.length !== b.length) return false;
  for (let i = 0; i < a.length; i++) {
    if (a[i] !== b[i]) return false;
  }
  return true;
}

async function connectSerial() {
  try {
    port = await navigator.serial.requestPort();
    await port.open({ baudRate: 115200 });
    writer = port.writable.getWriter();
    reader = port.readable.getReader();
    console.log("Conectado al puerto serial");
  } catch (err) {
    console.error("Error al conectar:", err);
  }
}

async function readSerial() {
  try {
    const { value, done } = await reader.read();
    if (done) {
      console.log("Puerto cerrado");
      reader.releaseLock();
      return;
    }
    if (value) {
      let str = new TextDecoder().decode(value);
      serialBuffer += str;
      if (serialBuffer.includes("\n")) {
        let lines = serialBuffer.split("\n");
        for (let i = 0; i < lines.length - 1; i++) {
          let cmd = lines[i].trim().toUpperCase();
          if (cmd) handleEvent(cmd);
        }
        serialBuffer = lines[lines.length - 1];
      }
    }
  } catch (err) {
    console.error("Error leyendo serial:", err);
  }
}


```

## Act 7

### Microbit
```
from microbit import *
import random

while True:
    if button_a.was_pressed():
        print("A")
    if button_b.was_pressed():
        print("B")
    if accelerometer.was_gesture("shake"):
        print("SHAKE")
    sleep(50)

```

[LINK AL EDITOR P5.JS](https://editor.p5js.org/Juandavid1612/full/UlvsIzVy2)

### Codigo p5.js

```
let port;
let writer;
let reader;

const CONFIG = "CONFIG";
const ARMED = "ARMED";
const EXPLODED = "EXPLODED";

let state = CONFIG;
let count = 20;
let startTime = 0;
let password = ['A', 'B', 'A'];
let keySequence = []; // antes se llamaba "key"
let keyIndex = 0;
let serialBuffer = "";

function setup() {
  createCanvas(400, 300);
  textAlign(CENTER, CENTER);
  textSize(24);
  startTime = millis();

  let connectBtn = createButton("Conectar Serial");
  connectBtn.position(10, 10);
  connectBtn.mousePressed(connectSerial);
}

function draw() {
  background(30);
  fill(255);
  text("Estado: " + state, width / 2, 50);
  text("Tiempo: " + count, width / 2, 100);

  if (state === CONFIG) {
    text("Aumentar: 'A'  Disminuir: 'B'  Armar: 'S'", width / 2, 200);
  } else if (state === ARMED) {
    text("Clave: A-B-A", width / 2, 200);
    if (millis() - startTime > 1000) {
      startTime = millis();
      count--;
      if (count <= 0) {
        state = EXPLODED;
      }
    }
  } else if (state === EXPLODED) {
    fill(255, 0, 0);
    text("💀 BOOM 💀", width / 2, height / 2);
    text("Reiniciar: 'R'", width / 2, 200);
  }

  if (reader) {
    readSerial();
  }
}

function keyPressed() {
  handleEvent(key.toUpperCase()); // "key" aquí es la tecla presionada, no el array
}

function handleEvent(ev) {
  if (state === CONFIG) {
    if (ev === 'A') {
      count = min(count + 1, 60);
    } else if (ev === 'B') {
      count = max(count - 1, 10);
    } else if (ev === 'SHAKE' || ev === 'S') {
      state = ARMED;
      startTime = millis();
    }
  } else if (state === ARMED) {
    if (ev === 'A' || ev === 'B') {
      keySequence[keyIndex] = ev;
      keyIndex++;
      if (keyIndex >= password.length) {
        if (arraysEqual(keySequence, password)) {
          count = 20;
          keyIndex = 0;
          keySequence = [];
          state = CONFIG;
        } else {
          keyIndex = 0;
          keySequence = [];
        }
      }
    }
  } else if (state === EXPLODED) {
    if (ev === 'RESET' || ev === 'R') {
      count = 20;
      state = CONFIG;
      keyIndex = 0;
      keySequence = [];
    }
  }
}

function arraysEqual(a, b) {
  if (a.length !== b.length) return false;
  for (let i = 0; i < a.length; i++) {
    if (a[i] !== b[i]) return false;
  }
  return true;
}

async function connectSerial() {
  try {
    port = await navigator.serial.requestPort();
    await port.open({ baudRate: 115200 });
    writer = port.writable.getWriter();
    reader = port.readable.getReader();
    console.log("Conectado al puerto serial");
  } catch (err) {
    console.error("Error al conectar:", err);
  }
}

async function readSerial() {
  try {
    const { value, done } = await reader.read();
    if (done) {
      console.log("Puerto cerrado");
      reader.releaseLock();
      return;
    }
    if (value) {
      let str = new TextDecoder().decode(value);
      serialBuffer += str;
      if (serialBuffer.includes("\n")) {
        let lines = serialBuffer.split("\n");
        for (let i = 0; i < lines.length - 1; i++) {
          let cmd = lines[i].trim().toUpperCase();
          if (cmd) handleEvent(cmd);
        }
        serialBuffer = lines[lines.length - 1];
      }
    }
  } catch (err) {
    console.error("Error leyendo serial:", err);
  }
}
