
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

## Explica por qué en la unidad anterior teníamos que enviar la información delimitada y además marcada con un salto de línea y ahora no es necesario.

En la unidad anterior, se usaba un protocolo de texto ASCII donde los datos (xValue, yValue, aState, bState) tenían una longitud variable. Para que el sketch pudiera saber cuándo terminaba un paquete y comenzaba el siguiente, se necesitaba un delimitador, que en este caso era el salto de línea (\n). Al recibir el \n, p5.js sabía que tenía un mensaje completo para procesar.

En el nuevo protocolo binario, el tamaño del paquete es fijo y conocido de antemano (6 bytes en el primer ejemplo, 8 bytes con el framing). Por lo tanto, el receptor simplemente tiene que leer un número fijo de bytes para obtener un paquete completo, sin necesidad de un delimitador. Esto hace que la comunicación sea más simple y eficiente, ya que no se envían bytes extra.

## Compara el código de la unidad anterior relacionado con la recepción de los datos seriales que ves ahora. ¿Qué cambios observas?
El código de p5.js ha cambiado significativamente en la forma en que lee los datos seriales:

if (port.availableBytes() >= 6): En lugar de esperar un delimitador (\n), el código verifica si hay al menos 6 bytes disponibles en el puerto serial.

let data = port.readBytes(6): Ahora se leen exactamente 6 bytes a la vez, garantizando que se recibe un paquete completo.

Se usa DataView para interpretar los bytes:

view.getInt16(0): Lee 2 bytes a partir del byte 0 como un entero con signo de 16 bits (xValue).

view.getInt16(2): Lee 2 bytes a partir del byte 2 como un entero con signo de 16 bits (yValue).

view.getUint8(4): Lee 1 byte a partir del byte 4 como un entero sin signo de 8 bits (aState). Se compara si es 1 para obtener el estado booleano.

view.getUint8(5): Lee 1 byte a partir del byte 5 como un entero sin signo de 8 bits (bState). Se compara si es 1 para obtener el estado booleano.

## ¿Qué ves en la consola? ¿Por qué crees que se produce este error?
El problema es de la sincronización Cuando se ejecuta el código y se observa la consola, se notara que los datos a veces se desalinean. Por ejemplo, en el resultado que muestras, aparecen valores extraños como 92 o 222 al inicio de las líneas, o valores incorrectos como microBitY: 513 o microBitX: 3073.

Este error se produce porque no hay un mecanismo de sincronización. El receptor asume que el primer byte que recibe es el inicio de un nuevo paquete. Sin embargo, si la comunicación se interrumpe o el programa de p5.js se inicia a mitad de una transmisión, el port.readBytes(6) podría leer los 6 bytes a partir del segundo, tercero, o cuarto byte del paquete real, lo que causaría que los datos se interpreten de forma incorrecta.

## ¿Qué cambios tienen los programas y ¿Qué puedes observar en la consola del editor de p5.js?

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
