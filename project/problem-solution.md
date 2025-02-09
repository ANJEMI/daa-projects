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

	while TeamsAreNotComplete():
	for number in S:
	    if sumA <= sumB:
	        A.append(number)
	        sumA += number
	    else:
	        B.append(number)
	        sumB += number

	return A, B

```

# Modelo de Programación Lineal para Formar Equipos Balanceados de Fútbol

A continuación se presenta un modelo de programación lineal adaptado para el problema original

## Variables de Decisión

- **Asignación de jugadores a equipos:**  
  Definimos una variable binaria \( x_{p,t} \) para cada jugador \( p \) y para cada equipo \( t \in \{1,2\} \):
  $$
  x_{p,t} =
  \begin{cases}
  1, & \text{si el jugador } p \text{ es asignado al equipo } t, \\
  0, & \text{en caso contrario.}
  \end{cases}
  $$
  
- **Diferencia de calidad entre equipos:**  
  Sea $$ d \ge 0 $$ una variable continua que representa la diferencia absoluta
  en la puntuación (o calidad) entre los dos equipos.

- **Puntaje de cada jugador:**  
  Sea $ P_p $ la puntuación o valor integral del jugador $ p $, que incorpora
  sus aptitudes físicas, mentales y su potencial sinérgico en el campo.

## Función Objetivo

El objetivo es **maximizar la calidad total** de ambos equipos a la vez que se
**minimiza la diferencia** en calidad entre ellos. Esto se puede formular
mediante una función objetivo de la forma:

$$
\text{Maximizar } Z = \underbrace{\left( \sum_{p} P_p \cdot x_{p,1} + \sum_{p} P_p \cdot x_{p,2} \right)}_{\text{Calidad total de los equipos}} - \lambda\, d,
$$

donde \(\lambda > 0\) es un parámetro que penaliza la diferencia en la calidad entre ambos equipos, promoviendo su balance.

## Restricciones

1. **Asignación Única de Jugadores:**  
   Cada jugador puede ser asignado a lo sumo a un equipo:
   $$ \forall p:\quad x_{p,1} + x_{p,2} \le 1.$$

2. **Restricción de Presupuesto:**  
   El costo total de los jugadores seleccionados (para ambos equipos) no debe
   exceder el presupuesto disponible. Sea $ \text{cost}_p $ el costo de
   contratar al jugador $ p $ y $ B $ el presupuesto total:
   $$
   \sum_{p} \text{cost}_p \cdot \left( x_{p,1} + x_{p,2} \right) \le B.
   $$
   *Alternativamente, si se dispone de un presupuesto por equipo, se impondría la restricción para cada equipo: $\forall t:\, \sum_{p} \text{cost}_p \cdot x_{p,t} \le B_t$.*

3. **Restricción de Origen (Club) de la Liga de Fantasía:**  
   Para evitar la sobreconcentración de jugadores de un mismo club, se impone que en cada equipo no se seleccionen más de 3 jugadores procedentes de cualquier club $ k $. Sea $ \mathcal{P}_k $ el conjunto de jugadores que pertenecen al club $ k $:
   $$
   \forall t \in \{1,2\},\; \forall k:\quad \sum_{p \in \mathcal{P}_k} x_{p,t} \le 3.
   $$
	*Esta es una reestricción común en ligas de fantasía*
4. **Restricciones de Composición por Posiciones:**  
   Cada equipo debe cumplir con una formación predefinida. Por ejemplo, si se desea que cada equipo cuente con 2 porteros, 5 defensores, 5 mediocampistas y 3 delanteros, se imponen las siguientes restricciones para cada equipo $ t $:
   - **Porteros:**
	$$
     \sum_{p \in \text{Porteros}} x_{p,t} = 2.
    $$
   - **Defensores:**
   	$$ 
     \sum_{p \in \text{Defensores}} x_{p,t} = 5.
    $$
   - **Mediocampistas:**
     $$
     \sum_{p \in \text{Mediocampistas}} x_{p,t} = 5.
     $$
   - **Delanteros:**
     $$
     \sum_{p \in \text{Delanteros}} x_{p,t} = 3.
     $$

5. **Balance entre Equipos:**  
   Para asegurar que ambos equipos sean lo más parejos posibles en términos de
   calidad, definimos las puntuaciones de cada equipo:
   $$
   p_1 = \sum_{p} P_p \cdot x_{p,1} \quad \text{y} \quad p_2 = \sum_{p} P_p \cdot x_{p,2}.
   $$
   Luego, se imponen las siguientes restricciones para definir la variable $ d $
   como la diferencia absoluta entre las puntuaciones:
   $$
   d \ge p_1 - p_2,\qquad d \ge p_2 - p_1.
   $$
   De esta forma, $ d \ge |p_1 - p_2| $, y al penalizar $ d $ en la función objetivo se fomenta la formación de equipos balanceados.


**Variables:**
- $ x_{p,t} \in \{0,1\} $ para cada jugador $ p $ y equipo $ t \in \{1,2\} $.
- $ d \ge 0 $ (diferencia de calidad entre equipos).

**Función Objetivo:**
$$
\text{Maximizar } Z = \left( \sum_{p} P_p \cdot (x_{p,1}+ x_{p,2}) \right) - \lambda\, d.
$$

**Sujeto a:**
1. $\forall p:\; x_{p,1} + x_{p,2} \le 1.$
2. \sum_{p} \text{cost}_p \cdot (x_{p,1}+ x_{p,2}) \le B.$
3. $\forall t \in \{1,2\},\; \forall k:\; \sum_{p \in \mathcal{P}_k} x_{p,t} \le 3.$
4. $\forall t \in \{1,2\}:$
   - $\sum_{p \in \text{Porteros}} x_{p,t} = 2,$
   - $\sum_{p \in \text{Defensores}} x_{p,t} = 5,$
   - $\sum_{p \in \text{Mediocampistas}} x_{p,t} = 5,$
   - $\sum_{p \in \text{Delanteros}} x_{p,t} = 3.$
5. Definición de puntajes:
   $$
   p_1 = \sum_{p} P_p \cdot x_{p,1},\quad p_2 = \sum_{p} P_p \cdot x_{p,2}.
   $$
6. Balance de equipos:
   $$
   d \ge p_1 - p_2,\qquad d \ge p_2 - p_1.
   $$

```python
from mip import Model, xsum, maximize, BINARY, CONTINUOUS

num_teams = 2
num_players = len(players_array)

model = Model(name="FPL_TwoTeams", solver_name="CBC")

# x[t][i] = 1 si el jugador i se asigna al equipo t (t = 0 o 1), 0 en caso contrario.
x = [
    [model.add_var(name=f"x({players_array[i].name},team={t+1})", var_type=BINARY)
     for i in range(num_players)]
    for t in range(num_teams)
]

d = model.add_var(name="d", var_type=CONTINUOUS, lb=0)

# Parámetro para penalizar el desbalance entre equipos.
lambda_penalty = 1  

team_points = [
    xsum(x[t][i] * players_array[i].total_points for i in range(num_players))
    for t in range(num_teams)
]

# Función Objetivo: Maximizar la suma total de puntos de ambos equipos
model.objective = maximize(
    xsum(team_points[t] for t in range(num_teams)) - lambda_penalty * d
)

# Restricción: Cada jugador puede ser asignado a lo sumo a un equipo.
for i in range(num_players):
    model.add_constr(xsum(x[t][i] for t in range(num_teams)) <= 1)

# Restricción de Presupuesto
BUDGET = 1000
model.add_constr(
    xsum(x[t][i] * players_array[i].now_cost for t in range(num_teams) for i in range(num_players))
    <= BUDGET
)

max_team_players = 3
clubs = {player.team for player in players_array}
for t in range(num_teams):
    for club in clubs:
        model.add_constr(
            xsum(x[t][i] * (players_array[i].team == club) for i in range(num_players))
            <= max_team_players
        )

# Restricciones de Posición
position_limits = {'GKP': 2, 'DEF': 5, 'MID': 5, 'FWD': 3}
for t in range(num_teams):
    for pos, limit in position_limits.items():
        model.add_constr(
            xsum(x[t][i] * (players_array[i].position == pos) for i in range(num_players))
            == limit
        )

model.add_constr(d >= team_points[0] - team_points[1])
model.add_constr(d >= team_points[1] - team_points[0])

status = model.optimize(max_seconds=120)

if status:
    for t in range(num_teams):
        print(f"\nEquipo {t+1}:")
        for i in range(num_players):
            if x[t][i].x >= 0.99:
                player = players_array[i]
                print(f" - {player.name} | Club: {player.team} | Posición: {player.position}")

```

## Enfoque con CGA y CKK
A continuación se muestra cómo se podrían adaptar el Complete Greedy Algorithm
(CGA) y el Complete Karmarkar–Karp algorithm (CKK) al problema inicial de formar
dos equipos de fútbol balanceados, y se explica qué ajustes son necesarios y
cuál es la complejidad temporal de cada uno.


## 1. Adaptación del Complete Greedy Algorithm (CGA)

### Idea Básica del CGA

- **CGA estándar:**  Considera todos los posibles particionamientos de un
conjunto de números mediante un árbol binario. Cada nivel del árbol representa
la decisión sobre en qué subconjunto colocar el siguiente número (generalmente
se ordenan en orden descendente). Se explora en orden de profundidad, y se
utiliza una heurística “greedy” (parecido a la versión anterior) que primero
explora la rama en la que el número se añade al subconjunto con menor suma.

### Ajustes Necesarios para el Problema de Equipos de Fútbol

1. **Ordenación y Selección de Jugadores:**  
   - Ordenar los jugadores por su “calidad” de forma descendente para que los
   jugadores de mayor impacto se asignen primero.
   - En el árbol, cada nivel representará la decisión sobre dónde asignar un
   jugador.

2. **Variables y Estado en la Exploración:**  
   - En cada nodo del árbol se debe llevar información no solo de la suma total
   de puntos de cada equipo, sino también de la **composición actual** (cantidad
   de jugadores por posición, jugadores por club y costo acumulado).  - De esta
   manera, antes de “extender” una rama se puede verificar si la asignación
   parcial es **factible** respecto a las restricciones.

3. **Heurística de Exploración:**  
   - Al decidir en cada nivel, primero se explora la rama en la que se asigna el
   jugador al equipo con la menor suma de puntos, siempre y cuando la asignación
   no infrinja ninguna restricción (por ejemplo, si ya se cumplió el cupo para
   esa posición o se excede el límite de jugadores de un club, se descarta esa
   rama).  - Si la asignación “greedy” resulta en una solución factible (aunque
   no óptima), se guarda y se sigue explorando el árbol en búsqueda de
   particiones aún más balanceadas.
   
### Complejidad Temporal del CGA

- **Espacio:**  
  Debido a la búsqueda en profundidad (DFS), se requiere O(n) espacio, donde _n_
  es el número de jugadores a particionar.

- **Tiempo:**  
  En el peor caso, se exploran hasta O(2^n) nodos. La presencia de restricciones
  adicionales puede, en la práctica, podar ramas no factibles y reducir la
  exploración, pero en el peor caso la complejidad sigue siendo exponencial.
  
---

## 2. Adaptación del Complete Karmarkar–Karp Algorithm (CKK)

### Idea Básica del CKK

- **CKK estándar:**  
  Este algoritmo también explora el espacio de particiones mediante un árbol
  binario, pero en cada nivel se consideran dos números. La rama izquierda
  corresponde a ponerlos en subconjuntos diferentes (reemplazándolos por su
  diferencia) y la derecha a ponerlos en el mismo subconjunto (reemplazándolos
  por su suma). Inicialmente se obtiene la solución mediante el método de
  “largest differencing”, y luego se explora para mejorar la solución.
  
### Ajustes Necesarios para el Problema de Equipos de Fútbol

1. **Adaptar la Operación de Diferencia y Suma:**  
   - En el problema de partición tradicional se combinan números mediante suma y
   diferencia. Para el problema de equipos, los “números” 	representan la
   calidad de los jugadores, pero además se deben **mantener atributos extra**:
   posición, club y costo.
   - En cada paso de la combinación se debe **actualizar un vector de estado** que incluya:
     - La diferencia en puntos entre los equipos.
     - Los contadores de jugadores por posición y por club en cada equipo.
     - El costo acumulado.
   
2. **Restricciones durante la Combinación:**  
   - Cuando se decide combinar dos jugadores (o conjuntos de jugadores), la rama
   que implique colocarlos en equipos distintos (izquierda) o en el mismo equipo
   (derecha) solo es viable si se puede seguir cumpliendo con las restricciones
   finales.  
   - Esto puede implicar descartar ramas que lleven a violaciones en la
   composición o en el presupuesto.

3. **Uso como Algoritmo Anytime:**  
   - CKK permite obtener rápidamente una solución (la obtenida por el método de
   largest differencing) y luego continuar buscando soluciones mejores. Esto es
   ventajoso en casos donde se requiere una solución “buena” en poco tiempo,
   aunque la búsqueda completa para la optimalidad pueda ser exponencial.

### Complejidad Temporal del CKK

- **Espacio:**  
  Al igual que CGA, se requiere O(n) espacio mediante búsqueda en profundidad.

- **Tiempo:**  
  El peor caso es O(2^n) en el número de combinaciones. Sin embargo, en muchas
  instancias aleatorias el algoritmo CKK es considerablemente más rápido que
  CGA. La incorporación de restricciones adicionales (composición por
  posiciones, presupuesto, club) implica realizar comprobaciones en cada rama,
  lo que puede aumentar el tiempo por nodo, pero en la práctica también ayuda a
  podar ramas inviables.  
  Además, CKK suele mostrar ventajas especialmente cuando existe una partición
  casi perfecta (equipos de muy similar calidad), pudiendo alcanzar mejoras de
  varios órdenes de magnitud en comparación con CGA. Esto es particularmente útil 
  en los problemas de ligas de fantasía porque normalmente valores con que ponderan 
  a los jugadores están acotados.

---

```python
function CKK(elements, best_solution):
    # elements: lista de "elementos" donde cada elemento representa
    # un conjunto parcial de jugadores con su estado acumulado para ambos equipos.
    # best_solution: solución global (mejor balance y máxima calidad).

    if length(elements) == 1:
        if is_feasible(elements[0].team1_state, elements[0].team2_state):
            current_diff = abs(elements[0].team1_state.total_points - elements[0].team2_state.total_points)
            if objective(elements[0].team1_state, elements[0].team2_state, current_diff) > best_solution.value:
                best_solution.value = objective(elements[0].team1_state, elements[0].team2_state, current_diff)
                best_solution.assignment = elements[0].assignment
        return elements[0].difference

    best_difference = INFINITY

    for each distinct pair (E_i, E_j) in elements:
        # Rama 1: Asignarlos a equipos opuestos
        new_element_diff = combine_different(E_i, E_j)
        if is_feasible(new_element_diff.team1_state, new_element_diff.team2_state):
            new_elements = elements.remove(E_i, E_j).add(new_element_diff)
            diff1 = CKK(new_elements, best_solution)
            best_difference = min(best_difference, diff1)
        
        # Rama 2: Asignarlos al mismo equipo
        new_element_sum = combine_same(E_i, E_j)
        if is_feasible(new_element_sum.team1_state, new_element_sum.team2_state):
            new_elements = elements.remove(E_i, E_j).add(new_element_sum)
            diff2 = CKK(new_elements, best_solution)
            best_difference = min(best_difference, diff2)

    return best_difference
```