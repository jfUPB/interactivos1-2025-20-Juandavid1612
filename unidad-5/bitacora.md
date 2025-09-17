
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


