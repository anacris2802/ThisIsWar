# This is War!

> El objetivo de este proyecto es simular distintos escenarios de batalla utilizando ecuaciones diferenciales implementadas en Python. 

---
## Contenido

* [Información general](#información-general)
* [Escenarios](#escenarios)
* [Técnicas usadas](#técnicas-usadas)
* [Ejemplo de uso](#ejemplo-de-uso)
    
    
---
## Información general
Para modelar las batallas, se usan las [*Leyes de Lanchester*]:

  <img src="https://render.githubusercontent.com/render/math?math=\frac{dx}{dt} = -ay ">
  
  <img src="https://render.githubusercontent.com/render/math?math=\frac{dy}{dt} = -bx ">
  
  con condiciones iniciales  <img src="https://render.githubusercontent.com/render/math?math=x(0) = x_0"> y  <img src="https://render.githubusercontent.com/render/math?math=y(0) = y_0">.
  
Los supuestos básicos del proyecto son los siguientes: 
* Los contrincantes son los azules y los rojos
* Los principales factores que deciden el resultado de la batalla son el número de tropas y el entrenamiento o equipo.
* El número de tropas azules y rojas son <img src="https://render.githubusercontent.com/render/math?math=x"> y <img src="https://render.githubusercontent.com/render/math?math=y"> respectivamente.
* La potencia de fuego azul y roja es <img src="https://render.githubusercontent.com/render/math?math=a"> y <img src="https://render.githubusercontent.com/render/math?math=b"> respectivamente.
* La potencia de fuego está basada en el entrenamiento, equipo, etc.


---
## Escenarios
Se tienen distintos escenarios, cada uno con sus supuestos particulares: 
1) [Combate sencillo](#combate-sencillo)
2) [Combate entre guerrillas](#combate-entre-guerrillas)
3) [Vietnam](#vietnam)
4) [Combate convencional](#combate-convencional)
5) [Esparta](#esparta)

Es importante mencionar que cada las ecuaciones varían dependiendo del escenario.

### Combate sencillo
En esta sección, los supuestos son los mismos a los generales. Las dos tropas conocen la ubicación del 'enemigo' y disparan con una potencia de fuego determinada previamente. 

### Combate entre guerrillas

Este modelo representa el combate de 2 fuerzas que no tienen la ubicación de la fuerza contraria. Así mismo, no conocen el daño que han infligido al equipo contrario. Esto hará que concentren su fuego en una área determinada en la que creen que se encuentra el enemigo.

### Vietnam
Este modelo es la unión del combate sencillo y del combate entre guerrillas. Una de las dos tropas es atacada por sorpresa, de manera que la fuerza que embosca tiene más ventaja sobre la emboscada. Particularmente representa la batalla de EE.UU. y Vietcong. Aquí, Vietcong tiene la ventaja de ser local y conocer el territorio por lo que toma el papel de fuerza emboscadora, mientras que EE.UU. es tomada por sorpresa. 

### Combate convencional

Aquí tomamos el supuesto de que las tropas pueden recibir refuerzos. Usamos una [función escalonada] para modelar el número de refuerzos que llegan en cierto periodo de tiempo. Gracias a esto, los resultados de las batallas se ven más cercanos a los que conocemos. 

### Esparta

Aquí se simula la batalla de Termópilas. Se supone que solo <img src="https://render.githubusercontent.com/render/math?math=C"> unidades de cada lado caben en el estrecho de Termópilas, por lo que las ecuaciones ahora solo toman el mínimo entre la cantidad de tropas <img src="https://render.githubusercontent.com/render/math?math=y"> y <img src="https://render.githubusercontent.com/render/math?math=C">.

---
## Técnicas usadas

Se tocan varios temas aparte del propósito principal que es modelar batallas:

* [Ecuaciones diferenciales](#ecuaciones-diferenciales)
* [Procesos a través del tiempo](#procesos-a-través-del-tiempo)
* [Gráficas interactivas](#graficas-interactivas)
* [Modelos basados en agentes](#modelos-basados-en-agentes)

Todos estos temas pudieron implementarse gracias a las siguientes librerías de Python:
* [NumPy](https://numpy.org/doc/stable/user/whatisnumpy.html)
* [SimPy](https://simpy.readthedocs.io/en/latest/)
* [Matplotlib](https://matplotlib.org/stable/index.html)
* [iPyWidgets](https://ipywidgets.readthedocs.io/en/stable/examples/Widget%20Basics.html)
* [IPython.display](https://ipython.org/ipython-doc/stable/api/generated/IPython.display.html)

### Ecuaciones diferenciales
El resolver [ecuaciones diferenciales de primer orden] es uno de los tópicos centrales. Para esto se usa el método `dsolve(ode)` incluido en la librería *SimPy*. Un ejemplo del código es el siguiente: 
```python
ode1 = Eq(dx,-a*y1(t))    # Declaramos la ecuación diferencial que queremos resolver
dsolve(ode1)              # Resolvemos con el método de SimPy

```
Ejecutando este código nos da el resultado de la `ode1` planteada. 

Es así como todo el proyecto puede resolverse de una manera práctica y rápida, sin el cansancio de tener que hacer todas las ecuaciones a mano. 


### Procesos a través del tiempo
Gracias a Python, el programar una función que nos explique y nos demuestre como se ve un proceso a través del tiempo es muy sencillo. 
Normalmente, los pasos que se siguen son los siguientes: 

![](https://github.com/anacris2802/Primavera-2021/blob/main/Comunicaci%C3%B3n%20Escrita/Images/Pasos_funci%C3%B3n_proyecto.jpg)

Siguiendo esta estructura general, el graficar un proceso a través de un tiempo t es relativamente sencillo. 

### Gráficas interactivas
Gracias a la librería *iPyWidgets* podemos jugar con los parámetros y darles el valor que nosotros queramos mientras se encuentren en el intervalo dado. 
Para usarlo, debemos tener una función en donde se de la orden de graficar cierta situación. Una vez con esto, usamos la función `interact()`:
```python
interact(plotModelo,a=(0,100),r=(0,100),pa=(0,100),pr=(0,100))
```

Dentro de `interact()` es donde se pone la función encargada de graficar, y los parámetros que se piden se pueden poner como intervalos o como valores fijos. Aquellos que tengan intervalos serán los valores que podremos modificar una vez ejecutado el programa. 


### Modelos basados en agentes
El modelado basado en agentes es una técnica que nos permite visualizar y simular con mayor claridad cómo las conductas individuales determinan la evolución de un sistema. 
En [este link] podemos conocer más acerca de esta técnica. 

Aquí se modeló una batalla cuerpo a cuerpo, en dónde a los soldados se les agregaron distintas variables, como son: valor de cohesión/miedo, en dónde si el valor pasa de un límite dado, el agente huye; posibilidades de herir, morir y matar; y finalmente, un atributo moral. 

---
## Ejemplo de uso
El código se encuentra en un Jupyter Notebook, por lo que es necesario ejecutarlo para que pueda funcionar. Una vez hecho esto, podemos observar que las gráficas interactivas cuentan con barras en la parte superior dónde podemos deslizar el punto para cambiar los valores:

![](https://github.com/anacris2802/Primavera-2021/blob/main/Comunicaci%C3%B3n%20Escrita/Images/imagen_proyecto.jpg)

Así, podemos personalizar las batallas y ver los distintos resultados que pueden ocasionar el tener menos o más tropas, menor o mayor potencia de ataque, etc. 


> ¡Espero disfrutes de jugar con las gráficas y los escenarios tanto como nosotros al crearlo! 


[*Leyes de Lanchester*]:https://es.wikipedia.org/wiki/Leyes_de_Lanchester#:~:text=Las%20leyes%20de%20Lanchester%20(en,fuego%20en%20funci%C3%B3n%20del%20tiempo.
[ecuaciones diferenciales de primer orden]:https://youtu.be/1n8Rv2eEVR0
[este link]:https://github.com/Skalas/Matematicas-computacionales-fall2020/blob/master/week9/0-agentes.ipynb
[función escalonada]:https://www.lifeder.com/funcion-escalonada/
