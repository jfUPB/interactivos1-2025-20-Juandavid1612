
# Evidencias de la unidad 6

## Seet seek

### Act 1

* ¿Qué ocurrió en la terminal cuando ejecutaste npm install? ¿Cuál crees que es su propósito?
* 
  <img width="807" height="171" alt="image" src="https://github.com/user-attachments/assets/fbfc3bd5-bbfb-48ab-9cf0-2f3b3c89db20" />

supongo que sirve para installar librerías o componentes de alguna librería

* ¿Qué mensaje específico apareció en la terminal después de ejecutar npm start? ¿Qué indica este mensaje?

" nodejs-test-1@1.0.0 start
node server.js

Server is listening on  http://localhost:3000"

* Describe lo que ves inicialmente en page1 y page2 en tu navegador.

  Cuando abrí solo page1 solo aparecía " esperando conexión de la otra ventana" al abrir la segunda tambien aparecieron dos circulos rojos con un punto en medio

* ¿Qué mensajes aparecieron en la terminal del servidor cuando abriste page1 y page2?
<img width="952" height="258" alt="image" src="https://github.com/user-attachments/assets/d70ba7fe-b529-4995-a0ef-1b38cfe2a2f9" />

* Describe qué sucede en ambas páginas del navegador cuando mueves una de las ventanas. ¿Cambia algo visualmente? ¿Qué mensajes aparecen (si los hay) en la consola del navegador (usualmente accesible con F12 -> Pestaña Consola) y en la terminal del servidor?

  Son dos circulos conectados con una linea en el medio, si muevo la ventana la cuerda sigue el centro de la que muevo siempre, de manera que están conectadas,  tambien si separo mucho las ventanas estas hacen coincidir la posición del otro circulo para que se vea siempre en lugar:

<img width="1620" height="990" alt="image" src="https://github.com/user-attachments/assets/6a29464b-6e8a-4917-957a-4014f5879371" />

<img width="900" height="783" alt="image" src="https://github.com/user-attachments/assets/82cfc9cb-fe6c-4a3f-97f6-af7f62b34dd9" />

Los mensajes del navegador dicen: Sync status: Synced 

Los mensajes del gitbash al parecer toma la posicion del circulo y las actualiza segun se vaya moviendo

<img width="550" height="147" alt="image" src="https://github.com/user-attachments/assets/25aa9757-7896-4257-9a7f-a12919742f12" />

<img width="925" height="966" alt="image" src="https://github.com/user-attachments/assets/72e1c381-b10d-4937-9fa6-9428f936ac9d" />

### Act 2

* ¿Qué es Internet?

  Sí claro que uso wifi, en mi casa uso siempre cable de red, el navegador sirve como una avenida, las paginas a las que accedo son calles secundarias y los apartados de estas paginas servirían como intersecciones, el que la "rampa" se corte significaría que el vehículo en sí mismo deje de funcionar, no habría ninguna forma de ir a ningun lado, es como si la carretera en este caso desapareciera por completo.

## Actividad 3 

### 🧐🧪✍️ Experimento 1

- **Intenta acceder a http://localhost:3000/page1. ¿Funciona?**

  - Si

- **Ahora intenta acceder a http://localhost:3000/pagina_uno. ¿Funciona?**

  - No

- ¿Qué te dice esto sobre cómo el servidor asocia URLs con respuestas? Restaura el código.

  - Pues me da a entender que es totalmente comprensible que el cliente esté en la dirección que debe estar para solicitar cierta información, mi predicción es que el cliente llega a la dirección que es, pide permiso con HTTP y despues de eso son puros WebSockets.

### 🧐🧪✍️ Experimento 2

- **Abre http://localhost:3000/page1 en una pestaña. Observa la terminal del servidor. ¿Qué mensaje ves? Anota el ID.**

  -  A user connected - ID: xf4drihvAkxir066AAAJ

- **Abre http://localhost:3000/page2 en OTRA pestaña. Observa la terminal. ¿Qué mensaje ves? ¿El ID es diferente?**

  - A user connected - ID: KS_ILIa6d863UpXyAAAN
 
- **Cierra la pestaña de page1. Observa la terminal. ¿Qué mensaje ves? ¿Coincide el ID con el que anotaste?**

  - User disconnected - ID: HSRvE1Gz8iNomJTeAAAL 

- **Cierra la pestaña de page2. Observa la terminal.**

  - User disconnected - ID: KS_ILIa6d863UpXyAAAN

### 🧐🧪✍️ Experimento 3

- **Inicia el servidor y abre page1 y page2.** 

- **Mueve la ventana de page1. Observa la terminal del servidor. ¿Qué evento se registra (win1update o win2update)? ¿Qué datos (Data:) ves?**

  -  Received win1update from ID: fWJWdmaFLYw2DpGrAAAB Data: { x: 937, y: 361, width: 958, height: 987 }

- **Mueve la ventana de page2. Observa la terminal. ¿Qué evento se registra ahora? ¿Qué datos ves?**

  - Received win2update from ID: dZsHSLSNiLGb-GJJAAAD Data: { x: 187, y: 22, width: 958, height: 987 }

- **Experimento clave: cambia socket.broadcast.emit(‘getdata’, page1); por socket.emit(‘getdata’, page1); (quitando broadcast). Reinicia el servidor, abre ambas páginas. Mueve page1. ¿Se actualiza la visualización en page2? ¿Por qué sí o por qué no? (Pista: ¿A quién le envía el mensaje socket.emit?). Restaura el código a broadcast.emit.**


### 📝 Actividad 4

**Abre page2.html en tu navegador (con el servidor corriendo). Abre la consola de desarrollador (F12). Detén el servidor Node.js (Ctrl+C). Refresca la página page2.html. Observa la consola del navegador. ¿Ves algún error relacionado con la conexión? ¿Qué indica? Vuelve a iniciar el servidor y refresca la página. ¿Desaparecen los errores?**
> Error que sale al desconectar:
<a name="Capturas04"></a>
<img width="700" height="58" alt="image" src="https://github.com/user-attachments/assets/60e39694-06eb-44ea-a208-b7cf803767cd" />

> Mensaje al reconectar: 
<img width="751" height="136" alt="image" src="https://github.com/user-attachments/assets/ad323b42-2884-450a-8f13-d43303c7ff4b" />
  
> Hay un error en el GET del script llamado `manager.js`. Efectivamente, este desaparece al volver a iniciar el servidor y refrescar. Lo que indica es que el cliente intenta conectarse al servidor pero no lo encuentra, mostrando que la comunicación depende completamente del servidor activo. Este es el script:  
<img width="829" height="583" alt="image" src="https://github.com/user-attachments/assets/681f460e-13f5-4ce7-8565-85bfa4de570e" />
  
**Comenta la línea socket.emit(‘win2update’, currentPageData, socket.id); dentro del listener connect. Reinicia el servidor y refresca page1.html y page2.html. Mueve la ventana de page2 un poco para que envíe una actualización. ¿Qué pasó? ¿Por qué?**
``
A user connected - ID: EYJDFhGbRZFCdga1AAAB
A user connected - ID: DQQRlrLOck6_rXLFAAAD
Received win1update from ID: DQQRlrLOck6_rXLFAAAD Data: { x: 0, y: 0, width: 808, height: 882 }
Debug - Connected clients: 2, Page1: 1, Page2: 0, Synced: 0
Sync status: pages=false, synced=false, clients=2
Debug - Connected clients: 2, Page1: 1, Page2: 0, Synced: 1
Sync status: pages=false, synced=false, clients=2
Debug - Connected clients: 2, Page1: 1, Page2: 0, Synced: 2
Sync status: pages=false, synced=true, clients=2
``
> Ambos clientes se conectaron, pero sólo se envió actualización de datos de la ventana 1. El debug indica que hay 2 clientes, pero solo está recibiendo datos de la página 1. En teoría están sincronizados, pero es que directamente no puede hacer la parte de dibujar la conexión porque no tiene la información necesaria para hacerlo.

**Asegúrate de tener este console.log en page2.js. Abre ambas páginas. Mueve la ventana de page1. Observa la consola del navegador de page2. ¿Qué datos muestra? Mueve la ventana de page2. Observa la consola de page1. ¿Qué pasa? ¿Por qué?**
> `Consola Page2:` Received valid remote data: {x: 100, y: 50, width: 800, height: 600}
> `Consola Page1:` Received valid remote data: { x: 937, y: 119, width: 159, height: 968 }
> Cada vez que una ventana se mueve, la otra recibe sus nuevos datos de posición en tiempo real.

**Observa checkWindowPosition() en page2.js y modifica el código del if para comprobar si el código dentreo de este se ejecuta. Mueve cada ventana y observa las consolas. ¿Qué puedes concluir y por qué?**
Agregué la línea `console.log('if funciona correctamente :)');` dentro del if. Cada frame que la ventana se moviera, se volvía a mandar el mensaje... lo que significa que el if se estaba ejecutando constantemente porque identificaba correctamente que las coordenadas anteriores eran distintas a las actuales. Mientras no se mueva, el mensaje no se manda. Por ende, ese código se corre solamente cuando es necesario... como el draw() en p5.js que se llama literal cada frame que corre el programa. Es más eficiente.    
<img width="824" height="471" alt="image" src="https://github.com/user-attachments/assets/9283cd30-e6e2-4667-b55d-0e748556ded9" />

<a name="BG04"></a>
**Cambia el background(220) para que dependa de la distancia entre las ventanas. Puedes calcular la magnitud del resultingVector usando let distancia = resultingVector.mag(); y luego usa map() para convertir esa distancia a un valor de gris o color. background(map(distancia, 0, 1000, 255, 0)); (ajusta el rango 0-1000 según sea necesario). Inventa otra modificación creativa.**
> La modificación creativa que hice fue agregarle un shake a la bolita para simular la tensión al alejarse y estirar la cuerda. Me quedó así :D
```program.js
function draw() {

    // MODIFICACIÓN DEL FONDO GRIS =================================

    //esto lo tuve que mover para arriba porque si no el fondo se dibujaba superpuesto al círculo
    let vector2 = createVector(remotePageData.x, remotePageData.y);
    let vector1 = createVector(currentPageData.x, currentPageData.y);
    let resultingVector = createVector(vector2.x - vector1.x, vector2.y - vector1.y);
    let distance = resultingVector.mag();

    let gris = map(distance, 0, 1000, 255, 0);
    background(gris);

    // MODIFICACIÓN CREATIVA :D ====================================
    // mapear la intensidad para un shake
    shakeIntensity = map(distance, 0, 1500, 0, 12);
    
    // hacerlo random
    let shakeX = random(-shakeIntensity, shakeIntensity);
    let shakeY = random(-shakeIntensity, shakeIntensity);
    
    //==============================================================
    
    if (!isConnected) {
        showStatus('Conectando al servidor...', color(255, 165, 0));
        return;
    }
    
    if (!hasRemoteData) {
        showStatus('Esperando conexión de la otra ventana...', color(255, 165, 0));
        return;
    }
    
    if (!isFullySynced) {
        showStatus('Sincronizando datos...', color(255, 165, 0));
        return;
    }
    // Solo dibujar cuando esté completamente sincronizado
    checkWindowPosition();
    
    // dibujar círculo main de la ventanita con el shake!!!!!!!!!!!!!!!!!!!!
    drawCircle(point2[0] + shakeX, point2[1] + shakeY);
    
    stroke(50);
    strokeWeight(20);
    drawCircle(resultingVector.x + remotePageData.width / 2, resultingVector.y + remotePageData.height / 2);
    line(point2[0] + shakeX, point2[1] + shakeY, resultingVector.x + remotePageData.width / 2, resultingVector.y + remotePageData.height / 2);
}
```



