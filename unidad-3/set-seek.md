# Unidad 3

## 🔎 Fase: Set + Seek


## Act 5 

### 1
```
from microbit import *
import utime


class Event:
    def __init__(self):
        self.current_event = "NONE"  # Sin vento al inicio 

    def set_event(self, ev):
        if self.current_event == "NONE": 
            self.current_event = ev

    def get_event(self):
        ev = self.current_event
        self.current_event = "NONE" 
        return ev



class BombTask:
    def __init__(self, event_source):
        self.event_source = event_source
        self.PASSWORD = ['A', 'B', 'A']
        self.key = []
        self.count = 20
        self.state = 'CONFIG'
        self.startTime = utime.ticks_ms()
        display.show(self.count, wait=False)

    def update(self):
        ev = self.event_source.get_event()

        if self.state == 'CONFIG':
            if ev == 'A':
                self.count = min(self.count + 1, 60)
                display.show(self.count, wait=False)
            elif ev == 'B':
                self.count = max(10, self.count - 1)
                display.show(self.count, wait=False)
            elif ev == 'SHAKE':
                self.startTime = utime.ticks_ms()
                self.state = 'ARMED'

        elif self.state == 'ARMED':
            # Cuenta regresiva
            if utime.ticks_diff(utime.ticks_ms(), self.startTime) > 1000:
                self.startTime = utime.ticks_ms()
                self.count -= 1
                display.show(self.count, wait=False)
                if self.count <= 0:
                    display.show(Image.SKULL)
                    self.state = 'EXPLODED'

            # Introducir clave
            if ev in ['A', 'B']:
                self.key.append(ev)
                if len(self.key) > len(self.PASSWORD):
                    self.key.pop(0)

                if self.key == self.PASSWORD:
                    self.count = 20
                    self.key = []
                    self.state = 'CONFIG'
                    display.show(self.count, wait=False)

        elif self.state == 'EXPLODED':
            if ev == 'RESET':
                self.count = 20
                self.state = 'CONFIG'
                self.key = []
                display.show(self.count, wait=False)



class ButtonTask:
    def __init__(self, event_source):
        self.event_source = event_source

    def update(self):
        if button_a.was_pressed():
            self.event_source.set_event('A')
        elif button_b.was_pressed():
            self.event_source.set_event('B')
        elif accelerometer.was_gesture('shake'):
            self.event_source.set_event('SHAKE')
        elif pin_logo.is_touched():
            self.event_source.set_event('RESET')



class SerialTask:
    def __init__(self, event_source):
        self.event_source = event_source

    def update(self):
        if uart.any():
            data = uart.read().decode('utf-8').strip().upper()
            if data in ['A', 'B', 'SHAKE', 'RESET']:
                self.event_source.set_event(data)



event = Event()
serial = SerialTask(event)
buttons = ButtonTask(event)
bomb = BombTask(event)

while True:
    serial.update()
    buttons.update()
    bomb.update()

```
### 2 


