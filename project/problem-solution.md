## Primeras intuiciones y abstracción del problema

Podemos darnos cuenta que para cada jugador podemos ponderar cada uno de sus atributos y la calidad de su relación con todos los demás jugadores, así como su nivel de complementación con las otras posiciones en el campo. Luego, podemos decir que tenemos una representación númerica de cada jugador que expresa su valor/capacidad etcétera. El problema se puede resumir a: dentro de un conjunto de jugadores separarlos en dos grupos cuya habilidad esté lo más equiparada posible, idealmente, que tenga exactamente el mismo potencial de rendimiento.

Este problema es un caso particular del problema siguiente:
_Dado un conjunto de números enteros, ¿existe una partición del conjunto en dos subconjuntos tales que la suma de los elementos en cada subconjunto sea exactamente la misma?_

### Demostración de que es $\mathcal{N} \mathcal{P}$-Completo

Definimos una instancia de este problema de partición como cualquier secuencia finita de números positivos. Digamos que $Y$ satisface el problema de Partición si y solo si los términos de $Y$ pueden ser particionados en dos subsecuencias, cada una con un total de la mitad del total de los términos de $Y$. La partición es claramente $\mathcal{NP}$ , ya que, dada la entrada, que serían los jugadores con su respectivo valor, y el certificado, que sería la instancia de la partición que podría satisfacer el problema, puede determinarse en tiempo polinómico si el certificado es válido o no (basta con realizar las operaciones aritméticas correspondientes y retornar `True` o `False`).

#### Reducción de Subset Sum a Partición

Definimos una instancia del problema Subset Sum como un par ordenado ($X$, $C$), donde $C$ es un número y $X$ es una secuencia de números positivos. El par ordenado ($X$, $C$) pertenece a Subset Sum si existe una subsecuencia de $X$ cuya suma es $C$.

Sea $R$ la siguiente reducción en tiempo polinomial:

Sea ($X$, $C$) una instancia del problema de suma de subconjuntos, donde **$X = x_1, ..., x_n$**.  
Sea $S = \sum_{i=1}^{n} x_i$. Sin pérdida de generalidad, asumimos que $0 \leq C \leq S$.
Definimos $R(X, C)$ como una secuencia $Y = y_1, \ldots, y_n, y_{n+1}, y_{n+2}$, donde:

$y_i = x_i $ para $ i \leq n$,
$y_{n+1} = C + 1$, y
$y_{n+2} = S - C + 1$.

La suma total de los elementos de $Y$ es:
$$\text{Total} = S + (C + 1) + (S - C + 1) = 2S + 2.$$

Para poder dividir $Y$ en dos grupos de igual suma, cada grupo debe sumar:
$$\frac{2S + 2}{2} = S + 1.$$

Entonces, $Y$ pertence a Partición si existen dos subsecuencias disjuntas de $Y$, cada una con una suma igual a $S + 1$.

#### Subset Sum si y solo si Particion

Supongamos que $(X, C)$ pertence a Subset Sum.
Sea $Z$ una subsecuencia de $X$ cuya suma es $C$. Entonces, $Z + \{y_{n+2}\}$ es una subsecuencia de $Y$ cuya suma es $S + 1$.

Por el contrario, supongamos que $Y$ se particiona en dos subsecuencias disjuntas, cada una con una suma de $S + 1$.
Ninguna de esas subsecuencias puede contener ambos últimos términos de $Y$, ya que su suma excede $S + 1$. Por lo tanto, una de las subsecuencias, digamos $W$, contiene $y_{n+2} = S - C + 1$.

Eliminamos $y_{n+2}$ de $W$ para obtener una subsecuencia de $X$ cuya suma es $C$.

Luego podemos concluir que si Subset Sum es $\mathcal{N} \mathcal{P}$-Completo entonces Partición es $\mathcal{N} \mathcal{P}$-Completo. $\blacksquare$

### Volviendo al Problema

Con esta reducción podemos ver que tenemos ante mano un problema intratable. Sin embargo esta modelación del problema solo nos permite determinar si existe una partición que satisfaga la "equidad" entre los conjuntos de jugadores del problema,la cuál no satisface todas las restricciones como: la cantidad de jugadores por equipo o el valor de contratación. Es necesario hacer algunas relajaciones del problema para poder ilustrar mejor nuestros requerimientos, entre ellos, fijar la cantidad de elementos por subsecuencia. Es decir, la pregunta cambiaría a en este conjunto de números finitos, ¿es posible construir dos subsecuencias de cardinalidad $k$ tal que la suma de sus elementos sumen la misma cantidad?
Si bien el problema ha sido relajado aún no se modela adecuadamente que se garantice el mejor espectáculo pues los conjuntos formados pueden no ser los mejores en calidad. Es aquí dónde se necesitará aplicar una nueva transformación para maximizar la calidad de los jugadores y el costo del evento. Por lo tanto ya no estamos en presencia de un problema de decisión, sino, de optimización.

La misma naturaleza del problema nos da una pista de una heurística bastante común en el diseño de algoritmos, un enfoque greedy (glotón). Este consiste en a medida que vayamos creando las subsecuencias escoger siempre el mayor valor disponible del conjunto con el objetivo de mantener en el mínimo posible la diferencia entre ambas subsecuencias en cada paso de la iteración. Si los números están ordenados entonces es posible formar dichas subsecuencias en $O(n)$ y en caso de no estarlo la complejidad sería $O(n\log{n})$. Para tener más clara la efectividad de este algoritmo es necesario analizar estadísticamente según la distribución de los números del conjunto para poder dar un estimado de cúanto variaría el valor de la suma de ambas subsecuencias.

### Pseudocódigo para el enfoque greedy en el problema de partición

1. **Entrada**: Conjunto de números $S$
2. **Salida**: Dos subsecuencias $A$ y $B$ que minimizan la diferencia entre sus sumas

```python
#pseudo codigo
A = []
B = []
sumA = 0
sumB = 0

function greedyPartition(S):

	sort(S, descending)

	for number in S:
	    if sumA <= sumB:
	        A.append(number)
	        sumA += number
	    else:
	        B.append(number)
	        sumB += number

	return A, B

```
