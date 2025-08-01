# Unidad 2

   
## Act 1

1.

La clase Pixel define un pixel con coordenadas y un intervalode tiempo, que determina cada cuanto cambia el estado entre encendido y apagado, en Init se guarda el estado inicial, el tiempo actual, se enciende o apaga el pixel y luego cambia a WaitTimeout, revisa si ha pasado el intervalo de tiempo definido, en caso tal se cambia el estado de pixel y actualiza el tiempo de inicio para contar otro intervalo


2.

Init y WaitTimeout

3.

Este programa no usa eventos externos, unicamente el paso del tiempo

4.

Encender o apagar un pixel, cambiar el estado del pixel, actualizar el tiempo


## Act 2

Estados: Rojo, Verde y Amarillo

Eventos: Paso del tiempo

Acciones: Encender solo el Led correspondiente, esperar a que pase el tiempo, actualizar el tiempo de referencia

```
from microbit import *
import utime


LED_ROJO = (0, 2)
LED_AMARILLO = (2, 2)
LED_VERDE = (4, 2)

class Semaforo:
    def __init__(self):
        self.estado = "Rojo"
        self.tiempo_inicio = utime.ticks_ms()
        self.intervalo = 3000  # 3 segundos por estado

    def encender_led(self, color):
     
        display.clear()
        if color == "Rojo":
            display.set_pixel(LED_ROJO[0], LED_ROJO[1], 9)
        elif color == "Amarillo":
            display.set_pixel(LED_AMARILLO[0], LED_AMARILLO[1], 9)
        elif color == "Verde":
            display.set_pixel(LED_VERDE[0], LED_VERDE[1], 9)

    def actualizar(self):
        tiempo_actual = utime.ticks_ms()
        if utime.ticks_diff(tiempo_actual, self.tiempo_inicio) > self.intervalo:
            self.tiempo_inicio = tiempo_actual  # Reiniciar tiempo

          
            if self.estado == "Rojo":
                self.estado = "Verde"
            elif self.estado == "Verde":
                self.estado = "Amarillo"
            elif self.estado == "Amarillo":
                self.estado = "Rojo"

        self.encender_led(self.estado)


semaforo = Semaforo()


while True:
    semaforo.actualizar()
    sleep(100)

```



