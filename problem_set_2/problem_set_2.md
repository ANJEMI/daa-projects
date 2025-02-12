# Problemas

# Como evitar que el Kevin vaya cana por delitos contra la humanidad

```python

def max_eliminations(K, officers):
    # Separate officers into left and right of Kevin
    left = []
    right = []
    for p in officers:
        if p < K:
            left.append(p)
        elif p > K:
            right.append(p)

    # Sort left in descending order to check consecutive towards Kevin
    left.sort(reverse=True)
    # Sort right in ascending order to check consecutive towards Kevin
    right.sort()

    max_left = 0
    current = K - 1
    for p in left:
        if p == current:
            max_left += 1
            current -= 1

    max_right = 0
    current = K + 1
    for p in right:
        if p == current:
            max_right += 1
            current += 1

    return max_left + max_right

# Example usage:
# Kevin's position
K = 0
# Officers' positions
officers = [1, 2, 3, -1, -2, -3]
print(max_eliminations(K, officers))  # Output: 6

```

La explicación es la siguiente: primero, se separan a los oficiales en dos
grupos, uno a la izquierda y otro a la derecha del Kevin, y se ordenan para
facilitar la revisión de posiciones consecutivas. Luego, se verifica cuántas
posiciones consecutivas hay empezando desde la que está justo al lado de Kevin,
contando los valores a medida que se avanza por las posiciones ordenadas. Al
final, se suman las longitudes de las cadenas contiguas más largas de ambos
lados para determinar cuántos oficiales se pueden eliminar. Este método es
eficiente porque se enfoca en las posiciones contiguas y tiene un rendimiento
óptimo, con una complejidad de tiempo de O(N log N) debido al proceso de
ordenamiento. (La suma de las longitudes de las cadenas contiguas más largas en
ambos lados da el número máximo de oficiales que se pueden eliminar. Este
enfoque asegura que capturamos las cadenas más largas posibles donde los
oficiales están bloqueados para moverse, lo que lleva a su eliminación.)

# Problema 12, Sistema. Oro Negro, los amigos acaballadores:

La idea es hacer un Sweep Line sobre eventos ordenados que sean la llegada de un amigo o el inicio del minuto final m-1, comprando el petróleo de forma greedy cuando haga falta y solo la cantidad necesaria, esperando la posibilidad de un mejor precio.
Se utiliza una estructura para saber dado un precio determinado el conjunto de amigos que ya llegaron que venden petróleo a ese precio y todavía les queda para vender. Insertar, modificar y eliminar en O(logn) y saber la menor llave en O(1).
Nos quedamos solo con los amigos que llegan antes del minuto m. Se ordenan los tiempos de llegada de los amigos restantes y se insertan en una cola de eventos.
Se recorre cronológicamente la cola manteniendo la cantidad de petroleo que quedaen cada momento. Si un evento es la llegada de un amigo, se inserta en el mapa de precios de petroleo a vector de amigos. Se determina el próximo evento a ocurrir, y se compra el petróleo justo que haga falta para llegar al próximo evento .
Si se llega al evento que representa el inicio del minuto final m-1 se logró.
La complejidad es O(nlogn) por ordenar a lo sumo n amigos por orden de llegada.

Costo mínimo de tener K litros en el min i

- dp[i][k] $ \forall j$ tal que $t_j = i$:
- $dp[i][min(C,k+ a_j)]$ =min dp[i][k]+ $P_j$
- dp[i+1][k-1] =min dp[i][k]
