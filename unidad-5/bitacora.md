
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
