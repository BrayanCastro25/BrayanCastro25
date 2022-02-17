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

- Sistemas subamortiguados (0 < ζ < 1).
- Sistemas criticamente amortiguados (ζ = 1).
- Sistemas sobreamortiguados (ζ > 1).

#### Sistemas subamortiguados con máximos sobreimpulsos

En el trabajo de la [FACET UNT](https://catedras.facet.unt.edu.ar/controldeprocesos/wp-content/uploads/sites/85/2016/02/T2.1-Identificacion.pdf), se plantearon varios métodos de identificación específicamente para sistemas de segundo orden.

![Identificación de sistemas subamortiguados con dos máximos sobre impulsos](/src/identificacion-sistemas-subamortiguados-dos-sobreimpulsos.png)

Obteniendo las constantes de identificación, se procede a calcular los parametros que representan a la función de transferencia:

![Calculo RA de máximos sobre impulsos](/src/calculo-ra-subamortiguado.png)

Obtenido el valor de 𝑅𝐴, se calcula el factor de amoriguamiento, con la siguiente ecuación:

![Calculo zeta de máximos sobre impulsos](/src/calculo-zeta-subamortiguado.png)

La frecuencia de oscilación, se obtiene:

![Calculo frecuencia de oscilación con máximos sobre impulsos](/src/calculo-wd-subamortiguado.png)

Ya obtenida 𝑊𝑑 se puede despejar la frecuencia natural del sistema 𝑊𝑛:

![Calculo frecuencia natural con máximos sobre impulsos](/src/calculo-wn-subamortiguado.png)

#### Método Smith

Este método se utiliza para identificar tanto en sistemas subamortiguados o sobre amortiguados, para ello se requiere ubicar el tiempo en dos puntos 20% y 60%. 

Donde 𝑊𝑛 se halla con la ecuación:

![Calculo wn método smith](/src/calculo-wn-smith.png)

En la siguiente gráfica, se aprecia que la relación entre los dos puntos son los que ayudan a obtener 𝑊𝑛 y ζ. Una limitante a tener en cuenta es el máximo factor de amortiguamiento a hallar es de ζ = 5.

![Método Smith segundo orden](/src/metodo-smith-segundo-orden.png)

#### Identificación sistema sobre amortiguado (Tratamiento de datos experimentales)

En el artículo web de [Weingaunity](https://hackaday.io/page/4829-identification-of-a-damped-pt2-system), identifica con gran precisión los parámetros 𝜁 y 𝑊𝑛 para sistemas sobre amortiguados, solo con obtener los tiempos en donde la salida llega a su 25% 𝑦 75% de la salida máxima, como se aprecia en la figura.

![Método de identificación Weingaunity](/src/metodo-identificacion-weingaunity.png)

Se requiere hallar los valores de 𝑇1 y 𝑇2, por medio de la siguiente serie de ecuaciones:

![Calculo relación r de Weingaunity](/src/calculo-r-weingaunity.png)

![Calculo parámetro p de Weingaunity](/src/calculo-p-weingaunity.png)

![Calculo parámetro x de Weingaunity](/src/calculo-x-weingaunity.png)

![Calculo parámetro T2 de Weingaunity](/src/calculo-t2-weingaunity.png)

![Calculo parámetro T1 de Weingaunity](/src/calculo-t1-weingaunity.png)

Finalmente, se hallan los valores de 𝑊𝑛 y ζ:

![Calculo Wn con método Weingaunity](/src/calculo-wn-weingaunity.png)

![Calculo zeta con método Weingaunity](/src/calculo-zeta-weingaunity.png)

En el trabajo presentado por Roberto Rivero 20 , argumenta la identificación de los sistemas de segundo orden. Por medio de otro tipo de clasificación, no el comúnmente conocido (subamortiguado, críticamente amortiguado y sobre amortiguado). Sino la presentada a continuación:

- Respuestas de sistemas oscilantes con sobre picos significativos (𝜻 < 𝟎.𝟓)

Estos sistemas se caracterizan por su fácil identificación de los dos primeros sobre impulsos.

![Método rivero sobre impulsos significativos](/src/metodo-rivero-sobreimpulsos-significativos.png)

Los parámetros que caracterizan se calculan de la siguiente manera:

![Calculo parámetro zeta por Rivero](/src/calculo-zeta-rivero.png)

![Calculo parámetro wn por Rivero](/src/calculo-wn-rivero.png)

- Respuestas de sistemas sin sobre picos o con sobre picos de poco valor (𝟎. 𝟓 ≤ 𝜻 ≤ 𝟐)

Para este grupo de sistemas, la identificación se rige por la toma de medida de los tiempos en los puntos determinados.

![Método rivero sobre impulsos de poco valor](/src/metodo-rivero-sobrepicos-poco-valor.png)

Se debe hallar la relación entre el t4 y t3:

![Relación entre t4 y t3](/src/relacion-t4-t3-rivero.png)

Obteniendo el valor de la relación, se procede a buscar en la siguiente tabla el valor (si el valor obtenido no se aproxima a ninguno de los establecidos, se puede realizar una interpolación para tener mayor precisión):

![Tabla relación t4 y t3](/src/tabla-rivero-sobrepicos-poco-valor.png)

De la tabla anterior se obtienen el valor de 𝜻 y Wnt4 requerido para la identificación de la frecuencia natural 𝑊𝑛:

![Calculo Wn sobrepicos con poco valor](/src/calculo-wn-sobrepicos-poco-valor.png)

- Respuestas de sistemas sobre amortiguados (𝜻 > 𝟐).

![Método RIvero sobreamortiguados](/src/metodo-rivero-sobreamortiguados.png)



![Placa electrónica para emular los múltiples sistemas de primer y segundo orden]()
