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

## Sintonización de controladores

A continuación, se disponen de los siguientes métodos para la sintonización de controladores.

### Sintonización de controladores PID para sistemas de primer orden

#### Primer método PID Ziegler & Nichols

La función de transferencia que representa a un controlador PID es la siguiente:

![Función de transferencia PID](/src/funcion-transferencia-pid.png)

![Constante Ki del PID](/src/constante-ki-pid.png)

![Constante Kd del PID](/src/constante-kd-pid.png)

Las constantes para el controlador Ziegler & Nichols, se halla con la siguiente tabla:

![Parámetros del controlador PID por Ziegler & Nichols](/src/parametros-pid-z&n.png)

#### Controlador PID mediante criterio Cohen Coon

Tiene el mismo criterio y procedimiento que el método Ziegler-Nichols variando cada una de las constantes del controlador.

![Parámetros del controlador PID por Cohen Coon](/src/parametros-pid-cohen-coon.png)

### Sintonización de controlador por síntesis directa para sistemas de primer y segundo orden

Este método es interesante, ya que a partir del diagrama de bloques de lazo cerrado se establece la función de transferencia para a partir de esta, poder elegir el comportamiento que se desee a la salida del sistema y obtener el controlador que requiere para dicho objetivo.

![Representacion lazo cerrado de síntesis directa](/src/representacion-lazo-cerrado-sintesis-directa.png)

Donde:

𝐶(𝑠): Función de transferencia del controlador.
𝐹(𝑠): Función de transferencia del sistema o proceso.
𝐺𝐿𝐶(𝑠): Función de transferencia del comportamiento del lazo cerrado.

La función de transferencia que se obtiene a la salida del lazo cerrado es:

![Función de transferencia del lazo cerrado por síntesis directa](/src/funcion-transferencia-lazo-cerrado.png)

La función de transferencia que requiere el controlador para cumplir el comportamiento deseado, es la siguiente:

![Función de transferencia del controlador síntesis directa](/src/funcion-transferencia-sintesis-directa.png)

## Diseño e implementación de placa electrónica emuladora de sistemas dinámicos

### Circuito sistemas dinámicos de primer orden

![Primer circuito sistemas primer orden](/src/primer-circuito-primer-orden.png)

Con este primer circuito se percibe el cambio en el 𝜏 pero modifica a la misma vez la ganancia del sistema, por este último motivo se decide probar otro circuito. Ya que la modificación de la ganancia se planea implementar con un circuito independiente que ajuste este parámetro para los dos sistemas.

Por ese motivo es necesario el circuito a continuación propuesto:

![Circuito final para sistemas primer orden](/src/circuito-final-primer-orden.png)

### Circuito sistemas dinpamicos de segundo orden

Para el sistema de segundo orden, se analiza el circuito filtro pasa bajas Sallen Key que fue implementado por Escobar Víctor y Arévalo Jerson.

![Circuito filtro Sallen Key de segundo orden](/src/circuito-filtro-sallen-key-segundo-orden.png)

Mediante las gráficas que se obtuvieron en la implementación del circuito se observa que el comportamiento de este circuito alcanza a presentar un sobre impulso es decir un sistema subamortiguado, pero debido a que se quiere representar varios sistemas para ponerlos en práctica. Se buscaron alternativas encontrando esta interesante implementación en la tesis de [Morales](https://repositorio.usm.cl/bitstream/handle/11673/46104/3560901064284UTFSM.pdf?sequence=1), donde se presentó un módulo interactivo para control de procesos.

![Circuito final sistemas de segundo orden](/src/circuito-final-segundo-orden.png)

## Diagrama de bloques implementación sistema completo

La placa electrónica fue diseñada con la herramienta KiCAD

![PCB en KiCAD](/src/circuito-en-kicad.png)

Y el diagrama de bloques que permite saber la configuración de todo el sistema se encuentra, a continuación:

![Diagrama del sistema completo](/src(diagrama-sistema-completo.png)

## Periodo de muestreo

### Periodo de muestreo sistemas de primer orden

Para estos tipos de sistemas se tiene que la frecuencia máxima es la inversa de la constante de tiempo.

![Frecuencia máxima sistemas de primer orden](frecuencia-maxima-primer-orden.png)

Donde 𝜏, se obtiene de la identificación, La frecuencia de Nyquist debe cumplir la inecuación:

![Frecuencia de Nyquist](/src/frecuencia-nyquist.png)

Y el periodo de muestreo finalmente es:

![Periodo de muestreo sistema primer orden](/src/periodo-muestreo-primer-orden.png)

### Periodo de muestreo sistemas de segundo orden

Para el periodo de muestreo de los sistemas de segundo orden, es necesario hallar el 𝜏 equivalente.

#### Constante de tiempo 𝜏 sistemas subamortiguados (𝜻 < 𝟏)

![Tau equivalente sistemas subamortiguados](/src/tau-equivalente-subamortiguado.png)

#### Constante de tiempo 𝜏 sistemas sobre amortiguados (𝜻 > 𝟏)

![Tau equivalente sistemas sobre amortiguados](/src/tau-equivalente-sobreamortiguado.png)

Lo que quiere decir que esos 𝜏𝑒𝑞 , operan de igual manera que él 𝜏 del sistema de primer orden.

## Ecuación recursiva

Con la ayuda de la transformada bilineal se puede llevar del tiempo continuo “s” al tiempo discreto “z”. Con la siguiente igualdad:

![Transformada bilineal](/src/transformada-bilineal.png)

### Ecuación recursiva controlador PID

Se parte de la ecuación del controlador en tiempo continuo "s":

![Función de transferencia PID](/src/funcion-transferencia-pid.png)

Realizando el reemplazo de "s", por medio de la transformada bilineal, se obtiene:

![Ecuación recursiva controlador PID](/src/ecuacion-recursiva-pid.png)

Donde:

![A0 de PID](/src/a0-pid.png)

![A1 de PID](/src/a1-pid.png)

![A2 de PID](/src/a2-pid.png)

![B0 de PID](/src/b0-pid.png)

### Ecuación recursiva síntesis directa de primer orden

La función en tiempo continuo es:

![Función de transferencia síntesis de controlador de primer orden](/src/funcion-sintesis-primer-orden.png)

Y su correspondiente ecuación recursiva es:

![Ecuación recursiva de síntesis de controlador de primer orden](/src/ecuacion-recursiva-primer-orden.png)

Donde:

![Valor de A síntesis directa primer orden](/src/a-sintesis-primer-orden.png)

![Valor de B síntesis directa primer orden](/src/b-sintesis-primer-orden.png)

![Valor de C síntesis directa primer orden](/src/c-sintesis-primer-orden.png)


### Ecuación recursiva síntesis directa de segundo orden

La función en tiempo continuo es:

![Función de transferencia síntesis de controlador de segundo orden](/src/funcion-sintesis-segundo-orden.png)

Donde 𝛿 es el factor de amortiguamiento deseado y 𝑊𝑜 es la frecuencia natural no amortiguada, deseada para dicho sistema de segundo orden posteriormente se calcula el controlador en tiempo discreto:

![Ecuación recursiva de síntesis de controlador de segundo orden](/src/ecuacion-recursiva-segundo-orden.png)

Donde:

![Valor de F síntesis directa segundo orden](/src/f-sintesis-segundo-orden.png)

![Valor de G síntesis directa segundo orden](/src/g-sintesis-segundo-orden.png)

![Valor de H síntesis directa segundo orden](/src/h-sintesis-segundo-orden.png)

![Valor de I síntesis directa segundo orden](/src/i-sintesis-segundo-orden.png)

![Valor de J síntesis directa segundo orden](/src/j-sintesis-segundo-orden.png)

![Valor de L síntesis directa segundo orden](/src/l-sintesis-segundo-orden.png)


## Interfaz humano-máquina (HMI)

### HMI encaragada del control del dispositivo

La interfaz encargada del control del dispositivo, es la siguiente:

![HMI encargada del control](/src/hmi-pantalla-control.png)

### HMI encargada de visualizar las gráficas

Y la otra Raspberry esta encargada de visualizar las gráficas, para su identificación del sistema dinámico y la implementación de su controlador correspondiente:

![HMI encargada de la visualización](/src/hmi-pantalla-grafica.png)

## Implementación del dispositivo

El dispositivo emulador implementado, se puede visualizar a continuación:

![Dispositivo emulador](/src/dispositivo-emulador.png)

### Proceso de configuración inicial del dispositivo

El dispositivo tiene tres (3) potenciometros en la parte frontal que permiten ajustar la constante de tiempo τ (Sistemas de primer orden), el factor de amortiguamiento ζ (Sistemas de segundo orden) y la ganancia del sistema K (Sistemas de primer y segundo orden). Luego se procede a seleccionar el tipo de lazo como "Abierto", ya que vamos a iniciar con el proceso de identificación del sistema. Para la selección del sistema se realiza con el interruptor que permite seleccionar el sistema de "Primer orden" o "Segundo orden" de manera opcional se pueden modificar el grosor y color de cada una de las gráficas y el fondo del gráfico, según las condiciones ambientales presentes o a gusto del usuario.

### Proceso de estimulación con escalón de entrada del sistema

Para la estimulación del sistema, se realiza con la ayuda de la pantalla de control del dispositivo configurando primeramente el valor del escalón que sera ingresado en la entrada del sistema, luego se presiona el botón "Iniciar" y posteriormente se aplica el setpoint con el botón "Setpoint" en la zona "Acción" y como opcional queda aplicar la perturbación al sistema, que representa el 10% del valor del setpoint. Al visualizar que el sistema converge en un valor se debe presionar el botón "Detener" que permite congelar la pantalla para su posterior identificación.

### Proceso de identificación del sistema

![Proceso de identificación del sistema](/src/proceso-identificacion-dispositivo.png)

La pantalla encargada de visualizar las gráficas, también permite a la hora de identificar el sistema, consultar el tiempo en determinado valor por medio de un deslizador o dos (2) botones para una mejor precisión y la opción de habilitar o no el cursor.

### Proceso de sintonización del controlador 

El proceso de sintonización del controlador se requiere hacer de manera manual, ya que es uno de los objetivos de este dispositivo facilitar su implementación pero fortalecer académicamente al estudiante en el área de control industrial. Para este proceso, es necesario primero "Limpiar" la pantalla, luego establecer el tipo de lazo "Cerrado", el tipo de controlador requerido, con los parámetros calculados para dicho sistema y el valor del setpoint e iniciar el proceso en lazo cerrado con el botón "Iniciar", despues de iniciado el proceso se aplica el "Setpoint" y opcional la "Perturbación". Finalmente se presiona el botón "Detener".

## Ejemplo de implementación del dispositivo

### Implementación lazo abierto con sistema subamortiguado

Ya establecidos los pasos a seguir para cada proceso, se demuestra un ejemplo con un sistema de segundo orden subamortiguado.

![Ejemplo lazo abierto sistema subamortiguado](/src/ejemplo-lazo-abierto-subamortiguado.png)

### Implementación pantalla de control por síntests directa para sistema subamortiguado

Se establece el tipo de controlador por síntesis directa para sistema de segundo orden.

![Ejemplo configuración lazo cerrado sistema subamortiguado](/src/ejemplo-configuracion-subamortiguado.png)

### Implementación lazo cerrado con sistema subamortiguado

Y la salida del lazo cerrado se muestra en la siguiente gráfica. En esta se pueden observar las cuatro señales "Entrada", "Salida", "Control" y "Perturbación".

![Ejemplo lazo cerrado sistema subamortiguado](/src/ejemplo-lazo-cerrado-subamortiguado.png)

### Implementación de sistema subamortiguado en lazo abierto frente a perturbaciones

Al aplicar la perturbación se aprecia como la salida del sistema oscila y aumenta su valor estacionario, ya que al no estar configurada como lazo cerrado, no permite corregir este imprevisto en el sistema.

![Ejemplo lazo abierto sistema subamortiguado frente a perturbaciones](/src/lazo-abierto-subamortiguado-perturbaciones.png)

### Implementación de sistema subamortiguado en lazo cerrado frente a perturbaciones

En cambio, al aplicar la perturbación en lazo cerrado, el controlador corrige inmediatamente esta entrada exterior evitando que cambie el valor estacionario.

![Ejemplo lazo cerrado sistema subamortiguado frente a perturbaciones](/src/lazo-cerrado-subamortiguado-perturbaciones.png)

