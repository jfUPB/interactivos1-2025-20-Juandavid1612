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


## Act 4



## Act 5

```
from microbit import *
import music
import utime

# Estados
CONFIG = 0
ARMED = 1
EXPLODED = 2

# Variables de estado
estado_actual = CONFIG
tiempo_config = 20  # tiempo inicial en segundos
cuenta_atras = tiempo_config
ultimo_tiempo = utime.ticks_ms()

# Constantes
TIEMPO_MIN = 10
TIEMPO_MAX = 60
INTERVALO_SEG = 1000  # milisegundos

def mostrar_tiempo(t):
    display.scroll(str(t), wait=False, loop=False)

def explosion():
    display.show(Image.SKULL)
    music.play(music.BA_DING)
    sleep(500)
    music.play(music.WAWAWAWAA)

while True:
    if estado_actual == CONFIG:
        mostrar_tiempo(tiempo_config)

        if button_a.was_pressed():
            if tiempo_config < TIEMPO_MAX:
                tiempo_config += 1

        if button_b.was_pressed():
            if tiempo_config > TIEMPO_MIN:
                tiempo_config -= 1

        if accelerometer.was_gesture("shake"):
            cuenta_atras = tiempo_config
            ultimo_tiempo = utime.ticks_ms()
            estado_actual = ARMED

    elif estado_actual == ARMED:
        tiempo_actual = utime.ticks_ms()
        if utime.ticks_diff(tiempo_actual, ultimo_tiempo) >= INTERVALO_SEG:
            cuenta_atras -= 1
            ultimo_tiempo = tiempo_actual
            mostrar_tiempo(cuenta_atras)

            if cuenta_atras <= 0:
                estado_actual = EXPLODED
                explosion()

    elif estado_actual == EXPLODED:
        display.show(Image.SKULL)

        if pin_logo.is_touched():
            estado_actual = CONFIG
            tiempo_config = 20
            cuenta_atras = tiempo_config
            display.clear()
            sleep(500)


```

### Vectores de prueba

** Vector de prueba 1: ** Aumentar el tiempo en modo configuración
Con la bomba en el estado CONFIG, se presiona el botón A una vez. Se espera que el valor del tiempo aumente en 1 segundo, siempre que no haya alcanzado el límite superior de 60 segundos. El estado del sistema debe permanecer en CONFIG y el nuevo tiempo debe mostrarse en el display.


** Vector de prueba 2: ** Disminuir el tiempo en modo configuración
En el estado CONFIG, se presiona el botón B una vez. El sistema debe reducir el tiempo configurado en 1 segundo, salvo que ya se encuentre en el límite inferior de 10 segundos. El estado debe seguir siendo CONFIG y el display debe actualizarse con el nuevo valor.

** Vector de prueba 3: ** Armar la bomba mediante el gesto de shake
Con el sistema en CONFIG y un tiempo válido ya configurado (por ejemplo, 25 segundos), se realiza el gesto de “shake”. Esto debe provocar la transición al estado ARMED, iniciando la cuenta regresiva desde el valor configurado. El display comenzará a mostrar el tiempo restante, reduciéndolo cada segundo.

** Vector de prueba 4: ** La bomba explota cuando el contador llega a cero
Con la bomba armada (estado ARMED) y la cuenta regresiva activa, se espera el paso del tiempo hasta que llegue a 0 segundos. En ese momento, el sistema debe cambiar al estado EXPLODED. Se debe mostrar un símbolo de explosión en el display (como una calavera) y sonar un efecto con el speaker.

** Vector de prueba 5: ** Reinicio tras explosión mediante el botón touch
Una vez que la bomba ha explotado y se encuentra en el estado EXPLODED, se toca el botón táctil (logo). El sistema debe reiniciarse: vuelve al estado CONFIG, se limpia el display, y el tiempo configurado se restablece automáticamente al valor inicial de 20 segundos.

