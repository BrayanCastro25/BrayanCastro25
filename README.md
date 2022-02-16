# Proyecto realizado en tesis de Ingeniero en Mecatrónica

El objetivo de este proyecto, fue integrar en un solo dispositivo, el proceso de identificación de múltiples sistemas dinámicos de primer y segundo orden (Emulados mediante amplificadores operacionales) y el proceso de sintonización de controladores PID clásico y/o avanzados. En este proyecto se hizo necesario el uso de los siguientes dispositivos (Hardware) y diferentes Software.

**Sistema Operativo**

- Rasphian

**Hardware**

- Raspberry Pi (x2).
- STM32F103C8T6 "Blue Pill" (X1).
- Pantallas tactiles (x2).
- Placa electrónica emuladora de los múltiples sistemas (x1).

**Software**

- KiCAD (Diseño de placa electrónica).
- Qucs (Simulación de circuitos).
- Xcos [Scilab] (Simulación de funciones de transferencia).
- PyQt5 [Python] (Programa para crear la HMI de cada una de las pantallas).
- STM32CubeIDE [Lenguaje C] (IDE utilizado para la programación de la STM32f103C8T6).

## Representación de sistemas dinámicos

### Representación de sistemas dinámicos de primer orden

La función de transferencia que representa a los sistemas de primer orden, se muestra a continuación:
![Dinámica de sistemas de primer orden](/src/dinamica-sistema-primer-orden.png)

### Representación de sistemas dinámicos de segundo orden

Para los sistemas de segundo orden, se representa con la siguiente función de transferencia:
![Dinámica de sistemas de segundo orden](/src/dinamica-sistema-segundo-orden.png)

## Métodos de identificación de sistemas dinámicos

Se plantearon los diferentes métodos de identificación de sistemas dinámicos de primer y segundo orden.

### Métodos de identificación para sistemas dinámicos de primer orden 

- Método de identificación de la tangente de Ziegler-Nichols.

![Método de identificación de la tangente de Ziegler-Nichols](/src/metodo-identificacion-Z&N.png)
<img src="https://latex.codecogs.com/png.image?\dpi{110}&space;\tau=at_1&plus;bt_2" title="\tau=at_1+bt_2" />

- Método de identificación dos puntos en general.

![Método de identificación dos puntos en general](/src/metodo-identificacion-dos-puntos-general.png)
<img src="https://latex.codecogs.com/png.image?\dpi{110}&space;t_o=ct_1&plus;dt_2" title="t_o=ct_1+dt_2" />

Por complejidad en el desarrollo del método de identificación de la tangente de Ziegler-Nichols, ya que este requiere graficar una recta tangente al punto de inflexión, en donde la exactitud del modelo depende del trazado de dicha recta. Se opta por el método de identificación de dos puntos en general.


### Métodos de identificación para sistemas dinámicos de segundo orden



![Placa electrónica para emular los múltiples sistemas de primer y segundo orden]()
