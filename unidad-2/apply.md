# Unidad 2


## 🛠 Fase: Apply

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

#### **Vector de prueba 1**: Aumentar el tiempo en modo configuración
Con la bomba en el estado CONFIG, se presiona el botón A una vez. Se espera que el valor del tiempo aumente en 1 segundo, siempre que no haya alcanzado el límite superior de 60 segundos. El estado del sistema debe permanecer en CONFIG y el nuevo tiempo debe mostrarse en el display.


#### **Vector de prueba 2:** Disminuir el tiempo en modo configuración
En el estado CONFIG, se presiona el botón B una vez. El sistema debe reducir el tiempo configurado en 1 segundo, salvo que ya se encuentre en el límite inferior de 10 segundos. El estado debe seguir siendo CONFIG y el display debe actualizarse con el nuevo valor.

#### **Vector de prueba 3:** Armar la bomba mediante el gesto de shake
Con el sistema en CONFIG y un tiempo válido ya configurado (por ejemplo, 25 segundos), se realiza el gesto de “shake”. Esto debe provocar la transición al estado ARMED, iniciando la cuenta regresiva desde el valor configurado. El display comenzará a mostrar el tiempo restante, reduciéndolo cada segundo.

**Vector de prueba 4:** La bomba explota cuando el contador llega a cero
Con la bomba armada (estado ARMED) y la cuenta regresiva activa, se espera el paso del tiempo hasta que llegue a 0 segundos. En ese momento, el sistema debe cambiar al estado EXPLODED. Se debe mostrar un símbolo de explosión en el display (como una calavera) y sonar un efecto con el speaker.

#### **Vector de prueba 5:** Reinicio tras explosión mediante el botón touch
Una vez que la bomba ha explotado y se encuentra en el estado EXPLODED, se toca el botón táctil (logo). El sistema debe reiniciarse: vuelve al estado CONFIG, se limpia el display, y el tiempo configurado se restablece automáticamente al valor inicial de 20 segundos.
