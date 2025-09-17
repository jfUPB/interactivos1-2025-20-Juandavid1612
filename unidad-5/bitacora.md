
# Evidencias de la unidad 5

# ACTIVIDAD 1

Describe cómo se están comunicando el micro:bit y el sketch de p5.js. ¿Qué datos envía el micro:bit?

 Se comunican por puerto serial utilizando el uart.

- El micro:bit usa uart.write(data) para enviar una línea de texto con datos separados por comas. Envia data = "{},{},{},{}\n".format(xValue, yValue, aState, bState)

- El sketch en p5.js usa la biblioteca p5.webserial.js para abrir el puerto serial, leer los datos y procesarlos en JavaScript.

¿Cómo es la estructura del protocolo ASCII usado?

xValue,yValue,aState,bState\n
Muestra y explica la parte del código de p5.js donde lee los datos del micro:bit y los transforma en coordenadas de la pantalla.

