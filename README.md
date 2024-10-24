# Sistema de Control de Temperatura con Arduino y ESP32

Este proyecto implementa un sistema de control de temperatura utilizando un Arduino que interactúa con un ESP32. El sistema regula la temperatura utilizando celdas Peltier y ventiladores para generar sensaciones de calor y frío, de acuerdo con la información recibida desde el ESP32. 

## Descripción

El sistema consta de:
- **Celdas Peltier** para generar calor o frío.
- **Ventiladores** para impulsar aire caliente o frío.
- **Sensores de temperatura LM35** conectados al Arduino para medir la temperatura.
- El ESP32 envía datos de temperatura y estado al Arduino, el cual actúa en consecuencia para controlar el ambiente.

El sistema implementa una **máquina de estados** que define tres estados principales:
1. **Estado 1 (Apagado):** Todos los dispositivos están apagados.
2. **Estado 2 (Calor):** El sistema activa la celda Peltier de calor y el ventilador de aire caliente hasta alcanzar la temperatura objetivo.
3. **Estado 3 (Frío):** El sistema activa la celda Peltier de frío y el ventilador de aire frío hasta alcanzar la temperatura objetivo.

## Componentes

- **Arduino Uno/Mega**
- **ESP32**
- **Sensores de temperatura (LM35)**
- **Celdas Peltier**
- **Ventiladores (Hot Air, Cold Air, Heatsink)**

## Conexiones

- **peltierHeat (Calor):** Pin 5
- **peltierCold (Frío):** Pin 6
- **Cartucho calefactor:** Pin 7
- **Ventilador de aire caliente:** Pin 8
- **Ventilador de aire frío:** Pin 9
- **Ventilador del disipador de calor:** Pin 10
- **Sensores de temperatura:** `A0` y `A1`

## Funcionalidades

### Comunicación Serial (Arduino <--> ESP32)

La función `comunicacion()` permite la interacción entre el ESP32 y el Arduino. El ESP32 envía un JSON con dos valores:
- **temperature:** Dato de temperatura objetivo.
- **state:** Estado del sistema (1: Apagado, 2: Calor, 3: Frío).

El Arduino analiza los datos recibidos, transforma el valor de temperatura a un `float`, y utiliza ese valor como setpoint en los controles de las celdas Peltier.

### Máquina de Estados

La función `stateMachine()` implementa una máquina de estados que controla el comportamiento del sistema:
- **Caso 1:** Apaga todos los componentes.
- **Caso 2:** Activa el sistema de calor y controla la temperatura con el sensor 1.
- **Caso 3:** Activa el sistema de frío y controla la temperatura con el sensor 2.

### Control de Temperatura

Las funciones `controlTemperatura1()` y `controlTemperatura2()` leen los valores de los sensores, calculan la temperatura en base al voltaje, y activan/desactivan las celdas Peltier y los ventiladores según sea necesario para alcanzar la temperatura objetivo.

## Instalación

1. **Conectar los componentes:** Conectar el Arduino, los sensores de temperatura, las celdas Peltier y los ventiladores de acuerdo a los pines definidos en el código.
2. **Configurar la comunicación con ESP32:** El ESP32 debe estar conectado al Arduino a través de los pines RX2 y TX2.
3. **Cargar el código en el Arduino:** Subir el código proporcionado al Arduino usando el IDE de Arduino.
4. **Configurar el ESP32:** Asegurarse de que el ESP32 esté enviando los datos en formato JSON correcto (temperatura y estado).

## Licencia
