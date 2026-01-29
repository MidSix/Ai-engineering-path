# Algoritmos del Tema 6: Exploración de Grafos

Este tema cubre las técnicas para recorrer grafos y las estrategias generales de búsqueda en espacios de estados: Backtracking y Ramificación y Poda.

---

## 1. Recorridos de Grafos

### 1.1 Recorrido en Profundidad (DFS)
Explora lo más lejos posible a lo largo de cada rama antes de retroceder.

#### Pseudocódigo (Recursivo)
```text
PROCEDIMIENTO dfsearch(G)
    PARA cada v en V HACER mark[v] <- no-visitado FIN PARA
    PARA cada v en V HACER
        SI mark[v] = no-visitado ENTONCES dfs(v) FIN SI
    FIN PARA
FIN PROCEDIMIENTO

PROCEDIMIENTO dfs(v)
    mark[v] <- visitado
    PARA cada w adyacente a v HACER
        SI mark[w] = no-visitado ENTONCES dfs(w) FIN SI
    FIN PARA
FIN PROCEDIMIENTO
```

#### Python
```python
def dfs(graph, start, visited=None):
    if visited is None:
        visited = set()
    visited.add(start)
    print(start) # Procesar nodo
    for neighbor in graph[start]:
        if neighbor not in visited:
            dfs(graph, neighbor, visited)
    return visited
```

**Complejidad:** $\Theta(n + m)$ con listas de adyacencia.

---

### 1.2 Recorrido en Anchura (BFS)
Visita todos los vecinos de un nodo antes de pasar al siguiente nivel.

#### Pseudocódigo
```text
PROCEDIMIENTO bfs(v)
    CrearCola(Q)
    mark[v] <- visitado
    Meter(v, Q)
    MIENTRAS Q no este vacia HACER
        u <- Sacar(Q)
        PARA cada w adyacente a u HACER
            SI mark[w] = no-visitado ENTONCES
                mark[w] <- visitado
                Meter(w, Q)
            FIN SI
        FIN PARA
    FIN MIENTRAS
FIN PROCEDIMIENTO
```

#### Python
```python
from collections import deque

def bfs(graph, start):
    visited = {start}
    queue = deque([start])
    while queue:
        u = queue.popleft()
        print(u) # Procesar nodo
        for neighbor in graph[u]:
            if neighbor not in visited:
                visited.add(neighbor)
                queue.append(neighbor)
    return visited
```

**Complejidad:** $\Theta(n + m)$ con listas de adyacencia.

---

## 2. Algoritmos de Retroceso (Backtracking)

Técnica para encontrar soluciones a problemas que satisfacen restricciones, explorando un árbol de búsqueda implícito (DFS en el espacio de estados).

### Esquema General (Backtracking)
```text
FUNCION Backtracking(estado_actual)
    SI es_solucion(estado_actual) ENTONCES
        procesar_solucion(estado_actual)
        RETORNAR
    FIN SI
    PARA cada opcion posible HACER
        SI es_valida(opcion) ENTONCES
            aplicar(opcion, estado_actual)
            Backtracking(estado_actual)
            deshacer(opcion, estado_actual) // Backtrack
        FIN SI
    FIN PARA
FIN FUNCION
```

#### Python (Template General)
```python
def backtracking(state, solutions):
    if is_solution(state):
        solutions.append(state.copy())
        return

    for option in get_options(state):
        if is_valid(option, state):
            state.append(option)      # Aplicar
            backtracking(state, solutions)
            state.pop()               # Deshacer (Backtrack)
```

### Ejemplo: El problema de la Mochila 0/1 (Discreta)
```text
FUNCION mochila_bt(i, peso_restante): valor
    SI i > n O peso_restante = 0 ENTONCES RETORNAR 0 FIN SI
    
    SI w[i] > peso_restante ENTONCES
        RETORNAR mochila_bt(i+1, peso_restante)
    SINO
        RETORNAR MAX(
            mochila_bt(i+1, peso_restante), // No tomar
            v[i] + mochila_bt(i+1, peso_restante - w[i]) // Tomar
        )
    FIN SI
FIN FUNCION
```

#### Python (Mochila)
```python
def knapsack_backtracking(i, current_weight, current_value, capacity, weights, values, n):
    # Caso base: no quedan objetos o mochila llena
    if i == n or current_weight == capacity:
        return current_value
        
    # Si el objeto no cabe, pasamos al siguiente
    if weights[i] > capacity - current_weight:
        return knapsack_backtracking(i + 1, current_weight, current_value, capacity, weights, values, n)
        
    # Ramificación: tomar vs no tomar
    # 1. Tomar el objeto
    with_item = knapsack_backtracking(i + 1, current_weight + weights[i], current_value + values[i], capacity, weights, values, n)
    
    # 2. No tomar el objeto
    without_item = knapsack_backtracking(i + 1, current_weight, current_value, capacity, weights, values, n)
    
    return max(with_item, without_item)
```

---

## 3. Ramificación y Poda (Branch and Bound)

Mejora el Backtracking (generalmente BFS o búsqueda con prioridad) utilizando **funciones de acotación** para "podar" ramas que no pueden conducir a una solución mejor que la ya encontrada.

### Características
- **Cota Local (CL):** Estimación del beneficio/coste del mejor nodo alcanzable desde el actual.
- **Cota Global (CG):** Valor de la mejor solución completa encontrada hasta el momento.
- **Poda:** Si $CL(nodo) \leq CG$ (en maximización), se elimina la rama.

### Esquema General
```text
FUNCION BranchAndBound()
    LNV <- CrearListaNodosVivos() // Cola de Prioridad
    CG <- Valor de una solución inicial (o -INFINITO)
    
    Meter(nodo_raiz, LNV)
    
    MIENTRAS LNV no vacia HACER
        x <- SacarMejor(LNV)
        
        SI CotaLocal(x) > CG ENTONCES // Solo si promete mejorar
            PARA cada hijo y de x HACER
                SI es_solucion(y) Y Valor(y) > CG ENTONCES
                    CG <- Valor(y)
                    MejorSolucion <- y
                SINO SI no es_hoja(y) Y CotaLocal(y) > CG ENTONCES
                    Meter(y, LNV)
                FIN SI
            FIN PARA
        FIN SI
    FIN MIENTRAS
    RETORNAR MejorSolucion
FIN FUNCION
```

#### Python (Template General)
```python
import heapq

class Node:
    def __init__(self, level, value, bound, ...):
        self.level = level
        self.value = value
        self.bound = bound
        # ... otros atributos del estado

    def __lt__(self, other):
        return self.bound > other.bound # Para Max-Heap (simulado con Min-Heap negando valores o invirtiendo lógica)

def branch_and_bound():
    # Priority Queue: almacena tuplas (-bound, node) para maximización
    pq = []
    
    # Crear nodo raíz
    root = Node(...)
    root.bound = calculate_bound(root)
    heapq.heappush(pq, root)
    
    max_profit = 0
    
    while pq:
        node = heapq.heappop(pq)
        
        # Poda si la cota no supera el mejor actual
        if node.bound <= max_profit:
            continue
            
        # Generar hijos
        # ... lógica específica del problema para ramificar ...
        
        for child in children:
            if child.is_solution and child.value > max_profit:
                max_profit = child.value
            
            child.bound = calculate_bound(child)
            if child.bound > max_profit:
                heapq.heappush(pq, child)
                
    return max_profit
```

---

## 4. Aplicaciones Especiales

### Ordenación Topológica (vía DFS)
En un DAG, si se añade el nodo a una lista al final del procedimiento `dfs(v)` (post-orden) y luego se invierte la lista, se obtiene una ordenación topológica.

```python
def topological_sort_dfs(graph):
    visited = set()
    stack = []
    
    def visit(u):
        if u not in visited:
            visited.add(u)
            for v in graph[u]:
                visit(v)
            stack.append(u)
            
    for node in graph:
        visit(node)
    return stack[::-1] # Invertir
```