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


## Act 3 

### 1

El programa permite simular concurrencia usando una máquina de estados y temporizadores. por lo que puede manejar botones y tiempos pasando de un estado a otroll sin bloquearse, realizando varias tareas de forma eficiente dentro de un único bucle continuo.

### 2 

#### Estados:

- STATE_INIT: Estado inicial (pseudoestado).

- STATE_HAPPY: Se muestra cara feliz.

- STATE_SMILE: Se muestra sonrisa.

- STATE_SAD: Se muestra cara triste.

#### Eventos: 

- Temporizador vence (time-out): Cuando se supera el intervalo del estado actual.

- Presión del botón A: Se detecta con button_a.was_pressed().

#### Acciones: 

- display.show(...): Muestra una imagen (feliz, sonrisa o triste).

- start_time = utime.ticks_ms(): Se reinicia el temporizador.

- interval = ...: Se actualiza el tiempo a esperar para el siguiente cambio de estado.


### 3

#### Vector de prueba 1:
Condición inicial: El sistema comienza en STATE_INIT.

Evento: Inicio del programa (no hay botón presionado).

Resultado esperado:

Muestra cara feliz (Image.HAPPY),

Cambia a STATE_HAPPY

Inicia temporizador con intervalo 1500 ms.

#### Vector de prueba 2:
Condición inicial: El sistema está en STATE_HAPPY.

Evento: El botón A es presionado antes de que pasen los 1500 ms.

Resultado esperado:

Se muestra cara triste (Image.SAD),

Cambia a STATE_SAD,

Intervalo cambia a 2000 ms.


#### Vector de prueba 3:
Condición inicial: El sistema está en STATE_SMILE.

Evento: No se presiona el botón, y pasa más de 1000 ms.

Resultado esperado:

Cambia a STATE_SAD,

Se muestra cara triste,

Intervalo cambia a 2000 ms.

