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
   - Aunque Max Clique es NP-hard, no pertenece a NP, ya que calcular el clique máximo no tiene una versión de decisión natural que permita verificar su solución en tiempo polinomial. Como el problema consiste en optimizar habría que probar "todas las soluciones" antes de confirmar que la solución actual es la mejor.

## Conclusión

El problema **Max Clique** es NP-hard, ya que resolverlo permite resolver cualquier instancia del problema NP-completo \( k \)-Clique en tiempo polinomial. Sin embargo, Max Clique **no es NP-completo**, pues no pertenece a NP.
