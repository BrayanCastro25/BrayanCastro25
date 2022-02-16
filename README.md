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

## Métodos de identificación de sistemas dinámicos

Se plantearon los diferentes métodos de identificación de sistemas dinámicos de primer y segundo orden.

### Métodos de identificación para sistemas dinámicos de primer orden 


- Método de identificación de la tangente de Ziegler-Nichols.

$\tau = at_1 +bt_2$

- Método de identificación dos puntos en general.

Por complejidad en el desarrollo del método de identificación de la tangente de Ziegler-Nichols, ya que este requiere graficar una recta tangente al punto de inflexión, en donde la exactitud del modelo depende del trazado de dicha recta. Se opta por el método de identificación de dos puntos en general.


![Placa electrónica para emular los múltiples sistemas de primer y segundo orden]()
