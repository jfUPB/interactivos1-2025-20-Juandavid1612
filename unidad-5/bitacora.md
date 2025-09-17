
# Evidencias de la unidad 5

# ACTIVIDAD 1

Describe cómo se están comunicando el micro:bit y el sketch de p5.js. ¿Qué datos envía el micro:bit?

 Se comunican por puerto serial utilizando el uart.

- El micro:bit usa uart.write(data) para enviar una línea de texto con datos separados por comas. Envia data = "{},{},{},{}\n".format(xValue, yValue, aState, bState)

- El sketch en p5.js usa la biblioteca p5.webserial.js para abrir el puerto serial, leer los datos y procesarlos en JavaScript.

¿Cómo es la estructura del protocolo ASCII usado?

- xValue,yValue,aState,bState\n
  
Muestra y explica la parte del código de p5.js donde lee los datos del micro:bit y los transforma en coordenadas de la pantalla.

```
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

```
Se lee una línea hasta \n y se separa por comas con .split(","). Luego, los valores se convierten a sus respectivos tipos: xValue y yValue se transforman en coordenadas (microBitX, microBitY) centradas en la pantalla, y aState y bState se convierten en valores booleanos. Finalmente, se llama a updateButtonStates() para detectar eventos de los botones.

¿Cómo se generan los eventos A pressed y B released que se generan en p5.js a partir de los datos que envía el micro:bit?

```
function updateButtonStates(newAState, newBState) {
 if (newAState === true && prevmicroBitAState === false) {
   // Botón A acaba de ser presionado
   lineModuleSize = random(50, 160);
   clickPosX = microBitX;
   clickPosY = microBitY;
   print("A pressed");
 }

 if (newBState === false && prevmicroBitBState === true) {
   // Botón B acaba de ser soltado
   c = color(random(255), random(255), random(255), random(80, 100));
   print("B released");
 }

 prevmicroBitAState = newAState;
 prevmicroBitBState = newBState;
}
```


- Se lee una línea hasta \n y se separa por comas con .split(","). Luego, los valores se convierten a sus respectivos tipos: xValue y yValue se transforman en coordenadas (microBitX, microBitY) centradas en la pantalla, y aState y bState se convierten en valores booleanos. Finalmente, se llama a updateButtonStates() para detectar eventos de los botones.

¿Cómo se generan los eventos A pressed y B released que se generan en p5.js a partir de los datos que envía el micro:bit?

```
function updateButtonStates(newAState, newBState) {
 if (newAState === true && prevmicroBitAState === false) {
   // Botón A acaba de ser presionado
   lineModuleSize = random(50, 160);
   clickPosX = microBitX;
   clickPosY = microBitY;
   print("A pressed");
 }

 if (newBState === false && prevmicroBitBState === true) {
   // Botón B acaba de ser soltado
   c = color(random(255), random(255), random(255), random(80, 100));
   print("B released");
 }

 prevmicroBitAState = newAState;
 prevmicroBitBState = newBState;
}
```
A pressed: Detecta cuando aState pasa de false a true.

B released: Detecta cuando bState pasa de true a false.

Capturas de pantalla de los algunos dibujos que hayas hecho con el sketch.

<img width="735" height="724" alt="image" src="https://github.com/user-attachments/assets/aa35471c-8a73-4e2c-a523-a5ba45b5b4f9" />

<img width="984" height="913" alt="image" src="https://github.com/user-attachments/assets/81eac808-8f28-413c-807e-75da279f97d7" />


# ACTIVIDAD 2
Captura el resultado del experimento anterior. ¿Por qué se ve este resultado?

- Al visualizar los mensajes binarios en texto (ASCII) se ve ilegible. Esto ocurre porque los datos en binario no representan letras ni numeros.

<img width="1001" height="263" alt="image" src="https://github.com/user-attachments/assets/795e2685-ebbe-4ea9-9438-bd2108981066" />

Captura el resultado del experimento anterior. Lo que ves ¿Cómo está relacionado con esta línea de código?

- Cuando ponemos la visualización a todo hex se pueden ver los 8 bytes que se estan enviando. 2h: xValue y yValue → 2 * 2 bytes = 4 bytes. 2B: aState y bState → 2 * 1 byte = 2 bytes. Esto son 6 bytes y al incluir n\ se añaden dos bytes más.
  
¿Qué ventajas y desventajas ves en usar un formato binario en lugar de texto en ASCII?

- La ventajas del formato binario son : Ocupa menos espacio, se transmiten menos datos por mensaje y no hay necesidad de analizar ni convertir a texto.
  
- las desventajas del formato binario: No es tan legible.
  
Captura el resultado del experimento. ¿Cuántos bytes se están enviando por mensaje? ¿Cómo se relaciona esto con el formato '>2h2B'? ¿Qué significa cada uno de los bytes que se envían?

- Se envian 6 bytes por mensaje
- Cada byte o grupo de bytes representa uno de los datos que el micro:bit está enviando.

  <img width="889" height="222" alt="image" src="https://github.com/user-attachments/assets/fa339d6b-1068-4484-add8-1392012de561" />

Diferencias:

- En ASCII los datos se ven como números separados por comas (fáciles de leer).

- En binario se ven como bytes en Hex (más compactos, pero difíciles de leer a simple vista).

Ventajas del binario:

- Ocupa menos espacio.

- Es más rápido de enviar.

- Ideal para sensores o aplicaciones con muchos datos.

Desventajas del binario:

- No se entiende a simple vista.

- Se necesita decodificarlo con código.

Ventajas del ASCII:

- Fácil de leer e interpretar por humanos.

- Útil para hacer pruebas rápidas.

Desventajas del ASCII:

- Ocupa más espacio (cada número puede tener varios caracteres).

- Es más lento al enviar y procesar.


# Actividad 03

### Explica por qué en la unidad anterior teníamos que enviar la información delimitada y además marcada con un salto de línea y ahora no es necesario.

Antes se usaba un protocolo de texto ASCII con delimitadores (salto de línea) para identificar los paquetes de datos. Ahora, con el protocolo binario, los paquetes tienen un tamaño fijo, lo que permite leerlos directamente sin delimitadores, haciendo la comunicación más simple y eficiente.

### Compara el código de la unidad anterior relacionado con la recepción de los datos seriales que ves ahora. ¿Qué cambios observas?
El código de p5.js ha cambiado significativamente en la forma en que lee los datos seriales:

if (port.availableBytes() >= 6): En lugar de esperar un delimitador (\n), el código verifica si hay al menos 6 bytes disponibles en el puerto serial.

let data = port.readBytes(6): Ahora se leen exactamente 6 bytes a la vez, garantizando que se recibe un paquete completo.

Se usa DataView para interpretar los bytes:

view.getInt16(0): Lee 2 bytes a partir del byte 0 como un entero con signo de 16 bits (xValue).

view.getInt16(2): Lee 2 bytes a partir del byte 2 como un entero con signo de 16 bits (yValue).

view.getUint8(4): Lee 1 byte a partir del byte 4 como un entero sin signo de 8 bits (aState). Se compara si es 1 para obtener el estado booleano.

view.getUint8(5): Lee 1 byte a partir del byte 5 como un entero sin signo de 8 bits (bState). Se compara si es 1 para obtener el estado booleano.

### ¿Qué ves en la consola? ¿Por qué crees que se produce este error?
El problema es de la sincronización Cuando se ejecuta el código y se observa la consola, se notara que los datos a veces se desalinean. Por ejemplo, en el resultado que muestras, aparecen valores extraños como 92 o 222 al inicio de las líneas, o valores incorrectos como microBitY: 513 o microBitX: 3073.

Este error se produce porque no hay un mecanismo de sincronización. El receptor asume que el primer byte que recibe es el inicio de un nuevo paquete. Sin embargo, si la comunicación se interrumpe o el programa de p5.js se inicia a mitad de una transmisión, el port.readBytes(6) podría leer los 6 bytes a partir del segundo, tercero, o cuarto byte del paquete real, lo que causaría que los datos se interpreten de forma incorrecta.

### ¿Qué cambios tienen los programas y ¿Qué puedes observar en la consola del editor de p5.js?

El framing como solución Con la implementación del framing, el código de micro:bit y p5.js cambia para asegurar una comunicación robusta:

Micro:bit:

Ahora envía un paquete de 8 bytes (b'\xAA' + data + bytes([checksum])).

El primer byte es un header (0xAA) que marca el inicio de cada paquete.

El último byte es un checksum, que sirve para verificar la integridad de los datos.

p5.js:

El código no lee los bytes de 6 en 6, sino que los va almacenando en un buffer (serialBuffer).

Utiliza un bucle while para buscar el byte de inicio (0xAA). Si el primer byte del buffer no es 0xAA, lo descarta y avanza.

Una vez que encuentra el header, verifica que haya un paquete completo (8 bytes) disponible.

Calcula el checksum de los datos recibidos y lo compara con el checksum enviado. Si no coinciden, descarta el paquete para evitar procesar datos corruptos.

Al ejecutar el código final, verás en la consola de p5.js que los datos se imprimen de forma consistente y correcta: microBitX: 500 microBitY: 524 microBitAState: true microBitBState: false. Esto confirma que el framing y el checksum solucionaron los problemas de sincronización e integridad de los datos.



# Actividad 04


Codigo a modificar

```
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
Codigo modificado

```
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



let port;
let connectBtn;
let microBitConnected = false;
let serialBuffer = []; // buffer para los bytes recibidos


const STATES = {
  WAIT_MICROBIT_CONNECTION: "WAIT_MICROBIT_CONNECTION",
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
    saveCanvas('microbit_screenshot_' + Date.now(), 'png');
    print("B pressed: screenshot taken!");
  }

  prevmicroBitAState = newAState;
  prevmicroBitBState = newBState;
}


function readSerialData() {
  let available = port.availableBytes();
  if (available > 0) {
    let newData = port.readBytes(available);
    serialBuffer = serialBuffer.concat(newData);
  }

  while (serialBuffer.length >= 8) {
    if (serialBuffer[0] !== 0xaa) {
      serialBuffer.shift(); // descarta hasta el header
      continue;
    }

    if (serialBuffer.length < 8) break;

    let packet = serialBuffer.slice(0, 8);
    serialBuffer.splice(0, 8);

    let dataBytes = packet.slice(1, 7);
    let receivedChecksum = packet[7];
    let computedChecksum = dataBytes.reduce((acc, val) => acc + val, 0) % 256;

    if (computedChecksum !== receivedChecksum) {
      console.log("Checksum error in packet");
      continue;
    }

    let buffer = new Uint8Array(dataBytes).buffer;
    let view = new DataView(buffer);

    microBitX = view.getInt16(0) + windowWidth / 2;
    microBitY = view.getInt16(2) + windowHeight / 2;
    microBitAState = view.getUint8(4) === 1;
    microBitBState = view.getUint8(5) === 1;

    updateButtonStates(microBitAState, microBitBState);
  }
}

function draw() {
  if (!port.opened()) {
    connectBtn.html("Connect to micro:bit");
    microBitConnected = false;
  } else {
    microBitConnected = true;
    connectBtn.html("Disconnect");
  }

  switch (appState) {
    case STATES.WAIT_MICROBIT_CONNECTION:
      if (microBitConnected === true) {
        print("Microbit ready to draw");
        strokeWeight(0.75);
        c = color(1, 1, 1);
        noCursor();
        port.clear();
        prevmicroBitAState = false;
        prevmicroBitBState = false;
        appState = STATES.RUNNING;
      }
      break;
    case STATES.RUNNING:
      if (microBitConnected === false) {
        print("Waiting microbit connection");
        cursor();
        appState = STATES.WAIT_MICROBIT_CONNECTION;
        break;
      }

     
      readSerialData();

      // Dibujar letras si A está presionado
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
  if (key == 's' || key == 'S') saveCanvas('screenshot_' + Date.now(), 'png');
  if (keyCode == DELETE || keyCode == BACKSPACE) background(255);
}

function keyPressed() {
  if (keyCode == UP_ARROW) angleDistortion += 0.1;
  if (keyCode == DOWN_ARROW) angleDistortion -= 0.1;
}

```
<img width="812" height="738" alt="image" src="https://github.com/user-attachments/assets/6c35abb3-7c6c-4e0f-8187-8809fd5b97a5" />
<img width="772" height="657" alt="image" src="https://github.com/user-attachments/assets/84ae145d-6da1-4fa9-bd8b-e137bfeabbe0" />


## AUTOEVALUACIÓN ##

| CRITERIOS | NOTA | JUSTIFICACIÓN|
|----------|----------|----------|
| 1. Profundidad de la Indagación  | 4   |Fui indagando según me surgieran dudas, apoyandome de mis compañeros que sí han asistido estas ultimas clases|
| 2. Calidad de la Experimentación | 4   | Comprobé que el código estuviera funcionando   |
| 3. Análisis y Reflexión | 3.5   | La bitácora conecta claramente la evidencia  con la explicación.|
| 4. Apropiación y Articulación de Conceptos | 3.5  |   Se plasmó en la bitácora el trabajo realizado tanto en clase como de manera asíncrónica para la entrega, no llegué a comprender todo honestamente por las inasistencias a clases  |
| TOTAL| 3.7|  Se pudo haber hecho mejor   |

