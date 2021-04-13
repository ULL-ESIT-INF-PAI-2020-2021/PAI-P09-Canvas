# Práctica 8. Programación Gráfica en JavaScript. HTML. La API Canvas.
### Factor de ponderación: 7

### Objetivos
Los objetivos de esta práctica son:
 
* Poner en práctica conceptos de Programación Gráfica en JavaScript usando la API Canvas.
* Practicar el uso de elementos HTML básicos.
* Practicar el uso de CSS para dotar de estilo a una página web simple.
* Poner en práctica metodologías y conceptos de Programación Orientada a Objetos en JavaScript.
* Practicar el proceso de pruebas de software (testing) utilizando Mocha y Chai.

### Rúbrica de evaluacion del ejercicio
Se señalan a continuación los aspectos más relevantes (la lista no es exhaustiva)
que se tendrán en cuenta a la hora de evaluar esta práctica:

* El comportamiento del programa debe ajustarse a lo solicitado en este enunciado.
* Deben usarse estructuras de datos adecuadas para representar los diferentes elementos que intervienen en el problema.
* Capacidad del programador(a) de introducir cambios en el programa desarrollado.
* Se comprobará que el código que el alumnado escribe se adhiere a las reglas de la Guía de Estilo.
* El código ha de estar documentado con [JSDoc](https://jsdoc.app/). 
  Haga que la documentación del programa generada con JSDoc esté disponible a través de una web alojada en su máquina IaaS de la asignatura.
* El alumnado ha de acreditar su capacidad para configurar y ejecutar ESLint en sus programas.
* El alumnado ha de acreditar que sabe depurar sus programas usando Visual Studio Code.
* Se comprobarán los tests unitarios que el alumnado ha desarrollado para todos sus códigos usando Mocha y Chai.
* El alumnado ha de acreditar su capacidad para ejecutar los tests unitarios desde la interfaz de Visual
  Studio Code.
* El programa debe ajustarse al paradigma de Orientación a Objetos.

### El conjunto de Mandelbrot

El [conjunto de Mandelbrot](https://en.wikipedia.org/wiki/Mandelbrot_set) 
es el conjunto de números resultante de repetidas iteraciones de la siguiente expresión:

`z = z^2 + c`   [1] 

donde `z` y `c` son números complejos. 
La función tiene la condición inicial `z = c`. 
Lo que normalmente se calcula es el número de iteraciones necesarias para que `z` alcance algún valor umbral, que en este caso es:

`|z| > 2.0`     [2]

Si, dentro de un número finito de iteraciones, se cumple la condición anterior, entonces se 
considera que el punto `c` está fuera del conjunto de Mandelbrot.

Como es sabido, un número complejo `c` puede representarse como un punto en un espacio bidimensional.

### La clase *Mandelbrot*
En esta práctica se propone desarrollar un módulo ES6 `mandelbrot.js` que implemente una clase `Mandelbrot` 
para visualizar el conjunto y calcular su área.

Previo a la implementación de la clase, diseñe y desarrolle un conjunto de tests para probar el correcto
funcionamiento de todos los métodos de la clase.

Encapsule la clase en un módulo que exporte la misma.

La visualización de la ejecución del programa se realizará a través de una página web alojada
en la máquina IaaS-ULL de la asignatura y cuya URL tendrá la forma:

[3] `http://10.6.129.123:8080/einstein-albert-mandelbrot.html`

en la que se incustará un canvas para dibujar el conjunto.
El valor del área y el error de la misma se imprimirán asimismo gráficamente dentro del canvas.
El resultado de dicha visualización debiera ser similar (a falta del área y error) al que muestra 
[esta página](https://upload.wikimedia.org/wikipedia/commons/2/21/Mandel_zoom_00_mandelbrot_set.jpg).

Diseñe asimismo otra página HTML simple 

[4] `http://10.6.129.123:8080/index.html`

que sirva de "página índice" para los ejercicios de la sesión de evaluación de la práctica.
La página [3] será uno de los enlaces de [4] y a su vez [3] tendrá un enlace "Home" que apunte a [4].

El cálculo del área del conjunto de Mandelbrot es un problema no trivial, ya que los resultados teóricos y 
numéricos obtenidos para este cálculo no concuerdan. 
Se propone usar el muestreo de Monte Carlo para calcular una solución numérica a este problema.
El método de Monte Carlo que que se propone implica la generación de un gran número de puntos 
aleatorios en el rango `[(-2.0, 0), (0.5, 1.125)]` del plano complejo. 
Cada punto será iterado usando la ecuación [1] hasta un determinado límite (digamos hasta 10000). 
Si dentro de ese número de iteraciones se cumple la condición de umbral, entonces ese punto se considera 
fuera (no perteneciente) del Conjunto de Mandelbrot. 
Al contabilizar el número de puntos aleatorios dentro del conjunto y los que están fuera, se obtiene
una buena aproximación del área del conjunto.
El algoritmo que se propone se describe a continuación:

1. Se genera un conjunto de `N`números complejos aleatorios en el intervalo `[(-2.0, 0), (0.5, 1.125)]`.
2. Realizar el muestreo de Monte Carlo iterando sobre los `N`puntos.  Para cada punto:

 - Asignar `z = c[i]`

 - Iterar según la ecuación [1], probando la condición umbral [2] en cada iteración:

 - Si no se cumple la condición del umbral, es decir, `|z| <= 2`, entonces repetir la iteración 
  (hasta un número máximo de iteraciones, digamos 10000). 
	Si después del número máximo de iteraciones la condición sigue sin satisfacerse, entonces 
	añada uno al número total de puntos dentro del conjunto.

 - Si se cumple la condición del umbral, entonces deje de iterar y pase al siguiente punto.
3. Una vez que todos los puntos han sido categorizados como dentro o fuera del conjunto, el área estimada y el error es dado por:

> Àrea = 2 * 2.5 * 1.125 * N<sub>dentro</sub> / N

> Error = Área / sqrt(N)

Escriba el código para calcular el área y su error.

Para visualizar el conjunto basta recorrer todos los píxeles (puntos) del canvas asignando un color a cada
uno dependiendo del número de iteraciones que precisa el punto en cuestión para determinarse si pertenece
o no al conjunto de Mandelbrot.
Es muy fácil hallar ejemplos de código que realizan este cálculo de diferentes formas.
Puede consultar 
[esta referncia](https://www.codingame.com/playgrounds/2358/how-to-plot-the-mandelbrot-set/adding-some-colors) 
(código en Python) a modo de ejemplo.

Pueden idearse estrategias para que la visualización del conjunto resulte fluída.
Un factor importante para conseguir fluidez es la optimización del código, puesto que se trata de una aplicación intensiva en cómputo.

Las siguientes deben tomarse como especificaciones de la aplicación a desarrollar:

* Todo el código estará ubicado en el directorio `src` del proyecto a desarrollar.
* Todo el código de los tests que realice para desarrollar la aplicación estarán ubicados en el directorio
* `test` de proyecto.
* El número de puntos `N` que el programa utiliza para calcular el área del conjunto es un 
  parámetro del programa que se introduce a través de una ventana emergente.
* La web mostrará un lienzo (canvas) que ocupe la mayor parte de una pantalla de ordenador de resolución usual.
* No ponga esfuerzo alguno en el diseño del código HTML ni CSS de las páginas. 
  Centre todos sus esfuerzos en la programación gráfica a través de la API canvas.

