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
