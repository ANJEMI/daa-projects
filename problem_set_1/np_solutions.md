# Problema: Hallar el clique de mayor tamaño en un grafo

## Análisis de complejidad

1. **Relación con el problema \( k \)-Clique:**

   - Sabemos que el problema \( k \)-Clique es NP-completo. Consiste en determinar si existe un clique de tamaño al menos \( k \) en un grafo \( G \).
   - Resolver el problema de hallar el clique de mayor tamaño (Max Clique) permite resolver el problema \( k \)-Clique de la siguiente manera:
     1. Usamos un algoritmo para Max Clique y calculamos el tamaño del clique máximo $k_{max}$ en \( G \).
     2. Comparamos $k\_{\text{max}}$ con $:
        - Si $k\_{\text{max}}$ $>=$ $k$ , entonces \( G \) cumple la propiedad del problema $ (k )$ clique. Pues si un grafo tiene un clique de tamaño $k$ también tiene clique de tamaño $j,  \forall j<=k$ .
        - En caso contrario, no se cumple pues no tiene clique del tamaño k.
   - Contradicción, resolver Max Clique en tiempo polinomial permitiría resolver \( k \) - Clique en tiempo polinomial. Por lo tanto como está demostrado que ( k ) - Clique es NP-Hard también Clique Máximo es NP-Hard.

2. **NP:**
   - Para que un problema sea NP-completo, debe cumplir dos condiciones:
     1. **Pertenecer a NP:** Su solución puede ser verificada en tiempo polinomial dado un certificado.
     2. **Ser NP-hard:** Cualquier problema en NP puede ser reducido a él en tiempo polinomial.
   - Aunque Max Clique es NP-hard, no pertenece a NP, ya que calcular el clique
   máximo no tiene una versión de decisión natural que permita verificar su
   solución en tiempo polinomial. Como el problema consiste en optimizar habría
   que probar "todas las soluciones" antes de confirmar que la solución actual
   es la mejor.
   
## Conclusión

El problema **Max Clique** es NP-hard, ya que resolverlo permite resolver
cualquier instancia del problema NP-completo \( k \)-Clique en tiempo
polinomial. Sin embargo, Max Clique **no es NP-completo**, pues no pertenece a
NP.


# Conjunto Dominante

## Definición

En un grafo \( G = (V, E) \), un conjunto de vértices \( D \subseteq V \) es un
**conjunto dominante** si cada vértice de \( V \) que no está en \( D \) es
adyacente a al menos un vértice en \( D \).

**Problema:** Determinar el conjunto dominante de menor cardinalidad en un
grafo \( G \).

## Pertenencia a NP

El problema **Conjunto Dominante** pertenece a NP porque, dado un conjunto
candidato \( D \), podemos verificar en tiempo polinomial si \( D \) domina
todos los vértices de \( G \).

## NP-Hardness

Podemos demostrar que **Conjunto Dominante** es NP-Hard mediante una reducción
desde el problema **Vertex Cover**, que se conoce que es NP-Completo.

### Demostraci�n: Reducci�n desde Vertex Cover

Dado un grafo \( G = (V, E) \) como entrada para el problema **Vertex Cover**,
construiremos un grafo \( G' = (V', E') \) tal que:

- \( V' \) contiene todos los vértices de \( V \) y, por cada arista \( (u, v)
\in E \), un nuevo vértice \( w \).
- \( E' \) incluye todas las aristas de \( E \) m�s dos nuevas aristas \( (u,
w) \) y \( (w, v) \) para cada arista \( (u, v) \) de \( E \).

La construcción de $ G' $ se realiza en tiempo polinomial, ya que solo
duplicamos las aristas y agregamos un nuevo vértice por cada arista original.

**Probemos que:** Si $ S $ es un vertex cover en $ G $, entonces $ S $
también es un conjunto dominante en $ G' $.

Como $ S $ es un vertex cover, cada arista en $ G $ tiene al menos un
extremo en $ S $. Consideremos $ v \in G' $. Si $ v $ es un nodo original
de $ G $, entonces o $ v \in S $ o debe existir alguna arista que conecte a
$ v $ a otro nodo $ u $. Como $ S $ es un vertex cover, si $ v \notin S $,
entonces $ u $ tiene que estar en $ S $, por lo que habría un nodo
adyacente a $ v $ en $ S $. Entonces $ v $ está cubierto por algún
elemento de $ S $. Por otra parte, si $ w $ es un nodo adicional en $ G' $,
entonces tiene dos nodos adyacentes $ u $ y $ v \in G $, y utilizando
el argumento anterior, al menos uno de ellos está en $ S $. Entonces los
nodos adicionales también están cubiertos por $ S $. Entonces, si $ G $
tiene un vertex cover, $ G' $ tendrá un conjunto dominante de al menos el
mismo tamaño.

**Probemos que:** Si $ D $ es un conjunto dominante de tamaño $ k $ de $ G $,
entonces en $ G' $ existe un vertex cover de tamaño a lo sumo $ k $.

Si $ D $ es un conjunto dominante de tamaño $ k $ en $ G' $, consideremos
todos los vértices adicionales $ w \in D $. Observemos que $ w $ debe estar
conectado exactamente a dos vértices $ u, v \in G $. Ahora, podemos
reemplazar $ w $ de manera segura por $ u $ o $ v $. El vértice $ w $
en $ D $ nos ayudará a dominar solo a $ u, v, w \in G' $. Sin embargo,
estas tres aristas forman un ciclo de 3 (**3-cycle**), por lo que podemos
elegir $ u $ o $ v $ y seguir dominando todos los vértices que $ w $
dominaba anteriormente. De esta manera, podemos eliminar todos los vértices
adicionales de la forma descrita. Dado que todos los vértices adicionales
corresponden a una de las aristas en $ G $, y dado que todos los vértices
adicionales están cubiertos por el conjunto $ D $ modificado, esto significa
que todas las aristas en $ G $ están cubiertas por el conjunto. Por lo tanto,
si $ G' $ tiene un conjunto dominante de tamaño $ k $, entonces $ G $
tiene un vertex cover de tamaño a lo sumo $ k $.

Así, hemos probado ambos lados de la equivalencia. Existe un conjunto dominante
de tamaño $ k $ en $ G' $ si y solo si existe un vertex cover de tamaño $
k $ en $ G $.

### Conclusión

Dado que sabemos que el problema **Vertex Cover** es NP-Completo, y demostramos
que es posible hacer una reducción en tiempo polinomial de **Vertex Cover** a
**Conjunto Dominante**, el problema **Conjunto Dominante** es NP-Hard.

## NP-Completitud

Como **Conjunto Dominante** es NP-Hard y pertenece a NP, concluimos que es
NP-Completo.

---

# Número Domatic

## Definición

El **número domatic** de un grafo \( G \), denotado como \( domatic(G) \), es
el número máximo \( k \) tal que los vértices de \( G \) pueden dividirse en \(
k \) conjuntos disjuntos \( D_1, D_2, \dots, D_k \), donde cada \( D_i \) es un
conjunto dominante.

**Problema:** Determinar el número domatic de un grafo \( G \).

## Pertenencia a NP

Al no conocer un algoritmo polinomial para verificar una solución del problema,
no podemos asegurar su pertenencia a NP.

## NP-Hardness

Demostraremos que **Número Domatic** es NP-Hard mediante una reducción desde
**3-SAT**.

### Demostración: Reducción desde 3-SAT

Sea \( \varphi = C_1 \land C_2 \land \dots \land C_m \) una instancia de
**3-SAT** con \( n \) variables \( x_1, \dots, x_n \). Construimos un grafo \(
G = (V, E) \) tal que:

1. Por cada cláusula \( C_i \in \{C_1, \dots, C_m\} \), existe un vértice \(
vc_i \).
2. Por cada variable \( x_j \in \{x_1, \dots, x_n\} \), existen tres vértices
\( a_j \), \( b_j \) y \( c_j \). Estos vértices forman un triángulo en el
grafo.
3. El vértice \( a_j \) corresponde al literal positivo \( x_j \), mientras que
el vértice \( b_j \) corresponde al literal negativo \( \neg x_j \).
4. Existe una arista entre \( vc_i \) y \( a_j \) si y solo si \( x_j \)
aparece en la cláusula \( C_i \).
5. Existe una arista entre \( vc_i \) y \( b_j \) si y solo si \( \neg x_j \)
aparece en la cláusula \( C_i \).
6. Existe un vértice \( r \) conectado a \( vc_i \) mediante un camino \( r \to
s_i \to vc_i \) para cada \( i \in \{1, \dots, m\} \).
7. Además, \( r \) está conectado directamente a \( a_j \) y \( b_j \) para
cada \( j \in \{1, \dots, n\} \).

**Probemos que:** La fórmula \( \varphi \) es satisfacible si y solo si el
grafo \( G \) tiene número domatic al menos 3.

- **\((\Rightarrow)\)** Supongamos que \( \varphi \) tiene una asignación
satisfactoria tal que, sin pérdida de generalidad, las variables \( x_1, \dots,
x_k \) son verdaderas y las variables restantes \( x_{k+1}, \dots, x_n \) son
falsas. Mostraremos que el número domatic de \( G \) es al menos 3.

  Dividimos el conjunto de vértices \( V \) en tres conjuntos disjuntos: \[ V_1
  = \{a_1, \dots, a_k, b_{k+1}, \dots, b_n\} \cup \{r\}, \] \[ V_2 = \{b_1,
  \dots, b_k, a_{k+1}, \dots, a_n, vc_1, \dots, vc_m\}, \] \[ V_3 = \{c_1,
  \dots, c_n, s_1, \dots, s_m\}. \]

  Es fácil verificar que cada uno de estos conjuntos es un conjunto dominante
  en \( G \). Por lo tanto, \( domatic(G) \geq 3 \).

- **\((\Leftarrow)\)** Supongamos que el grafo \( G \) tiene número domatic al
menos 3. Esto implica que \( domatic(G) = 3 \), ya que el vértice \( c_i \)
tiene grado 2 y puede ser dominado por como máximo tres conjuntos diferentes.

  Sea \( V_1, V_2, V_3 \subseteq V \) una partición de \( V \) tal que cada
  conjunto es dominante. Sin pérdida de generalidad, asumimos que \( r \in V_1
  \). Entonces, para cada \( i \in \{1, \dots, n\} \), sólo hay dos
  posibilidades:
  1. \( vc_i \in V_2 \) y \( s_i \in V_3 \).
  2. \( vc_i \in V_3 \) y \( s_i \in V_2 \).

  De lo contrario, \( s_i \) no podría ser dominado por uno de los conjuntos \(
  V_2 \) o \( V_3 \). Sin pérdida de generalidad, consideremos \( vc_i \in V_2
  \). Entonces, \( s_i \in V_1 \). Ahora, \( vc_i \) debe ser dominado por
  algún vértice en \( V_1 \). Por lo tanto, al menos uno de los vértices \( a_j
  \) o \( b_j \) conectados a \( vc_i \) debe estar en \( V_1 \). Por
  construcción, el conjunto \( V_1 \cap \{a_1, \dots, a_n, b_1, \dots, b_n\} \)
  representa una asignación satisfactoria para \( \varphi \).

  Además, para cada \( j \), no pueden elegirse ambos vértices \( a_j \) y \(
  b_j \) en \( V_1 \), ya que en ese caso \( c_j \) no sería dominado por \(
  V_2 \) ni \( V_3 \). Por lo tanto, la asignación es válida.

### Conclusión

Como **3-SAT** es NP-Completo, y hemos demostrado una reducción polinomial
hacia **Número Domatic**, concluimos que **Número Domatic** es NP-Hard.

## NP-Completitud

Como no podemos afirmar que **Número Domatic** sea NP, tampoco podemos afirmar
que sea NP-Completo.
