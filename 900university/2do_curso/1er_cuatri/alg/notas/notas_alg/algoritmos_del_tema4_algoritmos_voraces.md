# Algoritmos del Tema 4: Algoritmos Voraces

Esta nota cubre los algoritmos voraces estudiados, que toman decisiones localmente óptimas en cada paso con la esperanza de encontrar un óptimo global.

---

## 1. Problema de la Mochila (Fraccionario)

Dado un conjunto de objetos con peso $w_i$ y valor $v_i$, y una mochila con capacidad $W$, maximizar el valor total. En la versión fraccionaria (voraz), se pueden tomar partes de los objetos.

### Estrategia Voraz
Seleccionar objetos en orden decreciente de su **valor por unidad de peso** ($v_i / w_i$).

### Complejidad
- $\Theta(n \log n)$ debido a la ordenación inicial.

### Pseudocódigo
```text
FUNCION MochilaVoraz(W, pesos, valores, n)
    // Calcular ratios v_i / w_i para cada objeto
    // Ordenar objetos decrecientemente por ratio
    
    peso_actual <- 0
    valor_total <- 0
    
    PARA i <- 1 HASTA n HACER
        SI peso_actual + pesos[i] <= W ENTONCES
            // Tomar objeto completo
            peso_actual <- peso_actual + pesos[i]
            valor_total <- valor_total + valores[i]
        SINO
            // Tomar fracción del objeto
            peso_restante <- W - peso_actual
            valor_total <- valor_total + valores[i] * (peso_restante / pesos[i])
            RETORNAR valor_total // Mochila llena
        FIN SI
    FIN PARA
    RETORNAR valor_total
FIN FUNCION
```

### Python
```python
def knapsack_greedy(capacity, weights, values):
    n = len(weights)
    # Lista de tuplas (ratio, peso, valor)
    items = []
    for i in range(n):
        items.append((values[i] / weights[i], weights[i], values[i]))
    
    # Ordenar por ratio descendente
    items.sort(key=lambda x: x[0], reverse=True)
    
    total_value = 0
    current_weight = 0
    
    for ratio, weight, value in items:
        if current_weight + weight <= capacity:
            current_weight += weight
            total_value += value
        else:
            remaining = capacity - current_weight
            total_value += value * (remaining / weight)
            break
            
    return total_value
```

---

## 2. Ordenación Topológica

Para un grafo dirigido acíclico (DAG), es una ordenación lineal de sus vértices tal que para cada arista $(u, v)$, $u$ aparece antes que $v$.

### Algoritmo (Basado en Grado de Entrada)
1. Calcular el grado de entrada de cada nodo.
2. Añadir nodos con grado 0 a una cola.
3. Mientras la cola no esté vacía:
    - Extraer nodo $u$.
    - Añadir $u$ a la lista ordenada.
    - Para cada vecino $v$ de $u$, decrementar grado de entrada de $v$. Si es 0, añadir a cola.

### Complejidad
- $\Theta(n + m)$ usando listas de adyacencia (n: nodos, m: aristas).

### Pseudocódigo
```text
FUNCION OrdenacionTopologica(G) -> LISTA
    Calcular grados de entrada para cada nodo u en G
    Cola Q <- nodos con grado de entrada 0
    Lista L <- vacia
    
    MIENTRAS Q no este vacia HACER
        u <- Q.sacar()
        L.anadir(u)
        PARA cada vecino v de u HACER
            grado_entrada[v] <- grado_entrada[v] - 1
            SI grado_entrada[v] == 0 ENTONCES
                Q.meter(v)
            FIN SI
        FIN PARA
    FIN MIENTRAS
    
    SI longitud(L) == numero_nodos(G) ENTONCES
        RETORNAR L
    SINO
        RETORNAR Error (Ciclo detectado)
    FIN SI
FIN FUNCION
```

### Python
```python
def topological_sort(graph):
    # graph es un diccionario {u: [v1, v2...]}
    # Calcular grados de entrada
    in_degree = {u: 0 for u in graph}
    for u in graph:
        for v in graph[u]:
            in_degree[v] = in_degree.get(v, 0) + 1
            
    queue = [u for u in in_degree if in_degree[u] == 0]
    topo_order = []
    
    while queue:
        u = queue.pop(0)
        topo_order.append(u)
        
        if u in graph:
            for v in graph[u]:
                in_degree[v] -= 1
                if in_degree[v] == 0:
                    queue.append(v)
                    
    if len(topo_order) != len(graph):
        return None # Hay un ciclo
    return topo_order
```

---

## 3. Árbol de Recubrimiento Mínimo (MST)

Dado un grafo conexo, no dirigido y ponderado, encontrar un árbol que conecte todos los vértices con el peso total mínimo.

### 3.1 Algoritmo de Kruskal
Selecciona las aristas de menor peso que no formen ciclos.

#### Estrategia
1. Ordenar todas las aristas por peso ascendente.
2. Inicializar cada nodo en un conjunto disjunto propio.
3. Iterar aristas ordenadas $(u, v)$:
    - Si $u$ y $v$ están en conjuntos diferentes (find), unirlos (union) y añadir arista al MST.

#### Complejidad
- $O(m \log m)$ o $O(m \log n)$ dependiendo de la estructura de datos.

#### Pseudocódigo
```text
FUNCION Kruskal(G) -> ConjuntoAristas
    MST <- vacio
    Ordenar aristas de G por peso ascendente
    Para cada vertice v: MakeSet(v)
    
    PARA cada arista (u, v) en orden HACER
        SI Find(u) != Find(v) ENTONCES
            Agregar (u, v) a MST
            Union(u, v)
        FIN SI
    FIN PARA
    RETORNAR MST
FIN FUNCION
```

#### Python
```python
class UnionFind:
    def __init__(self, n):
        self.parent = list(range(n))
        self.rank = [0] * n
    
    def find(self, i):
        if self.parent[i] != i:
            self.parent[i] = self.find(self.parent[i])
        return self.parent[i]
    
    def union(self, i, j):
        root_i = self.find(i)
        root_j = self.find(j)
        if root_i != root_j:
            if self.rank[root_i] > self.rank[root_j]:
                self.parent[root_j] = root_i
            else:
                self.parent[root_i] = root_j
                if self.rank[root_i] == self.rank[root_j]:
                    self.rank[root_j] += 1
            return True
        return False

def kruskal(n, edges):
    # edges es lista de tuplas (u, v, peso)
    # n es numero de vertices (0 a n-1)
    mst = []
    uf = UnionFind(n)
    
    # Ordenar aristas por peso
    edges.sort(key=lambda x: x[2])
    
    for u, v, weight in edges:
        if uf.union(u, v):
            mst.append((u, v, weight))
            
    return mst
```

### 3.2 Algoritmo de Prim
Hace crecer el MST desde un nodo inicial, añadiendo siempre la arista de menor peso que conecta un nodo del árbol con uno fuera.

#### Estrategia
1. Mantener un array `distancia` (coste mín para conectar al árbol) y `padre`.
2. Usar una cola de prioridad para extraer el nodo más cercano no visitado.

#### Complejidad
- $O(m \log n)$ con montículo binario.
- $O(n^2)$ con array (bueno para grafos densos).

#### Pseudocódigo
```text
FUNCION Prim(G, inicio) -> MST
    Para cada v en G: 
        distancia[v] <- INFINITO
        padre[v] <- NULO
    distancia[inicio] <- 0
    Q <- ColaPrioridad con todos los vertices (clave: distancia)
    
    MIENTRAS Q no este vacia HACER
        u <- Q.extraer_min()
        PARA cada vecino v de u HACER
            SI v en Q Y peso(u, v) < distancia[v] ENTONCES
                distancia[v] <- peso(u, v)
                padre[v] <- u
                Q.actualizar_prioridad(v, distancia[v])
            FIN SI
        FIN PARA
    FIN MIENTRAS
    RETORNAR {(v, padre[v]) | v en G - {inicio}}
FIN FUNCION
```

#### Python (Versión simplificada)
```python
import heapq

def prim(graph, start_node):
    mst = []
    visited = set([start_node])
    edges = [
        (cost, start_node, to)
        for to, cost in graph[start_node].items()
    ]
    heapq.heapify(edges)
    
    while edges:
        cost, frm, to = heapq.heappop(edges)
        if to not in visited:
            visited.add(to)
            mst.append((frm, to, cost))
            for to_next, cost2 in graph[to].items():
                if to_next not in visited:
                    heapq.heappush(edges, (cost2, to, to_next))
    return mst
```

---

## 4. Caminos Mínimos: Algoritmo de Dijkstra

Encuentra el camino más corto desde un nodo origen a todos los demás en un grafo con pesos **no negativos**.

### Estrategia
1. Inicializar distancias a $\infty$ (0 para origen).
2. Usar cola de prioridad con (distancia, nodo).
3. Mientras cola no vacía:
    - Extraer nodo $u$ con menor distancia.
    - Para cada vecino $v$ de $u$:
        - Si $dist[u] + peso(u,v) < dist[v]$:
            - Actualizar $dist[v]$.
            - Insertar/Actualizar $v$ en cola.

### Complejidad
- $O(m \log n)$ con montículo binario.
- $O(n^2)$ con array simple.

### Pseudocódigo
```text
FUNCION Dijkstra(G, inicio) -> Distancias
    Para cada v en G:
        distancia[v] <- INFINITO
        padre[v] <- NULO
    distancia[inicio] <- 0
    Q <- ColaPrioridad con todos los vertices (clave: distancia)
    
    MIENTRAS Q no este vacia HACER
        u <- Q.extraer_min()
        PARA cada vecino v de u HACER
            SI distancia[u] + peso(u, v) < distancia[v] ENTONCES
                distancia[v] <- distancia[u] + peso(u, v)
                padre[v] <- u
                Q.actualizar_prioridad(v, distancia[v])
            FIN SI
        FIN PARA
    FIN MIENTRAS
    RETORNAR distancia
FIN FUNCION
```

### Python
```python
import heapq

def dijkstra(graph, start):
    distances = {node: float('infinity') for node in graph}
    distances[start] = 0
    pq = [(0, start)]
    
    while pq:
        d, u = heapq.heappop(pq)
        
        if d > distances[u]:
            continue
        
        for v, weight in graph[u].items():
            if distances[u] + weight < distances[v]:
                distances[v] = distances[u] + weight
                heapq.heappush(pq, (distances[v], v))
                
    return distances
```