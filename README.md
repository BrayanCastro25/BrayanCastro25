# Dispositivo portátil emulador de dinámicas de procesos de primer y segundo orden para su identificación y correspondiente sintonización del controlador.


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

#### Método de identificación de la tangente de Ziegler-Nichols.
 
Estos métodos se basan en trazar una tangente en el punto de inflexión de la respuesta de salida del sistema. La constante de tiempo del sistema 𝜏 por este método se calcula desde donde inicia la tangente hasta donde finaliza, es decir, donde se cruza con el valor 𝑦 = 0 y 𝑦 = 𝑦𝑚á𝑥. La ganancia estática del sistema es la fracción entre la diferencia de salida y la diferencia de entrada del sistema. Por último, el 𝑡 𝑜 es el tiempo que hay entre el inicio del escalón y el inicio de la tangente.

![Método de identificación de la tangente de Ziegler-Nichols](/src/metodo-identificacion-Z&N.png)

Para calcula la constante de tiempo de la función se procede a realizar con la siguiente ecuación:

![Ecuación para calcular la constante tau](/src/ecuacion-tau.png)
<< \tau=at_1+bt_2 >>

Y para calcular el tiempo muerto del sistema se realiza con la siguiente ecuación:

![Ecuación para calcular el tiempo muerto](/src/ecuacion-to.png)

#### Método de identificación dos puntos en general.

Existe este tipo de método más estandarizado, ya que el anterior requiere realizar una recta tangente al punto de inflexión, en donde la exactitud del modelo depende del trazado de dicha recta, este tipo de método permite la identificación de parámetros por medio de dos puntos que se especifican por medio de porcentajes de la señal de salida.

![Método de identificación dos puntos en general](/src/metodo-identificacion-dos-puntos-general.png)

Los valores de las constantes a, b, c y d, se identifican de manera general en la la siguiente tabla:

![Constantes para identificación dos puntos en general](/src/constantes-metodos-dos-puntos-general.png)

Por complejidad en el desarrollo del método de identificación de la tangente de Ziegler-Nichols, ya que este requiere graficar una recta tangente al punto de inflexión, en donde la exactitud del modelo depende del trazado de dicha recta. Se opta por el método de identificación de dos puntos en general.

### Métodos de identificación para sistemas dinámicos de segundo orden

Para los sistemas dinámicos de segundo orden, se debe tener en cuenta que estos sistemas se clasifican en tres (3) tipos dependiendo del factor de amortiguamiento (ζ):

#### Sistemas subamortiguados (0 < ζ < 1)

En el trabajo de la [FACET UNT](https://catedras.facet.unt.edu.ar/controldeprocesos/wp-content/uploads/sites/85/2016/02/T2.1-Identificacion.pdf), se plantearon varios métodos de identificación específicamente para sistemas de segundo orden.

![Identificación de sistemas subamortiguados con dos máximos sobre impulsos](/src/identificacion-sistemas-subamortiguados-dos-sobreimpulsos.png)

Obteniendo las constantes de la función de transferencia 


![Placa electrónica para emular los múltiples sistemas de primer y segundo orden]()
