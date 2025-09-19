# Unidad 3

## 🔎 Fase: Set + Seek


## Act 5 

<img width="1433" height="732" alt="image" src="https://github.com/user-attachments/assets/5ebf9ad7-34ad-408d-9789-b3feb7e646de" />

### 2 

| Estado inicial | Evento disparador      | Acciones                                                                                   | Estado final |
| -------------- | ---------------------- | ------------------------------------------------------------------------------------------ | ------------ |
| CONFIG         | A (botón o serial)     | Incrementar tiempo (máx. 60) y mostrarlo en pantalla                                       | CONFIG       |
| CONFIG         | B (botón o serial)     | Disminuir tiempo (mín. 10) y mostrarlo en pantalla                                         | CONFIG       |
| CONFIG         | SHAKE (botón o serial) | Guardar tiempo, iniciar cuenta regresiva                                                   | ARMED        |
| ARMED          | A/B (clave correcta)   | Añadir tecla a secuencia, si coincide con `A-B-A` reinicia tiempo y vuelve a configuración | CONFIG       |
| ARMED          | A/B (clave incorrecta) | Añadir tecla a secuencia, si no coincide con clave, reiniciar secuencia                    | ARMED        |
| ARMED          | Tiempo = 0             | Mostrar calavera, pasar a estado de explosión                                              | EXPLODED     |
| EXPLODED       | RESET (logo o serial)  | Reiniciar contador a 20 y limpiar clave                                                    | CONFIG       |



