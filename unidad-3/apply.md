# Unidad 3


## 🛠 Fase: Apply


## Act 6

```
let serial;
let portName = 'COM3';

const CONFIG = "CONFIG";
const ARMED = "ARMED";
const EXPLODED = "EXPLODED";

let state = CONFIG;
let count = 20;
let startTime = 0;
let password = ['A', 'B', 'A'];
let key = [];
let keyIndex = 0;

function setup() {
  createCanvas(400, 300);
  textAlign(CENTER, CENTER);
  textSize(24);

  serial = new p5.SerialPort();
  serial.list();
  serial.open(portName);
  serial.on('data', serialEvent);

  startTime = millis();
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
}

function serialEvent() {
  let data = serial.readLine().trim().toUpperCase();
  if (data.length > 0) {
    handleEvent(data);
  }
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

function keyPressed() {
  handleEvent(key.toUpperCase());
}

function arraysEqual(a, b) {
  if (a.length !== b.length) return false;
  for (let i = 0; i < a.length; i++) {
    if (a[i] !== b[i]) return false;
  }
  return true;
}

```
