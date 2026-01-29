# Algoritmos del Tema 2: Estructuras de Datos

Esta nota cubre las operaciones fundamentales de las estructuras de datos estudiadas: Árboles Binarios de Búsqueda, Montículos, Conjuntos Disjuntos y Tablas de Dispersión.

---

## 1. Árboles Binarios de Búsqueda (ABB)

Estructura jerárquica donde para cada nodo, los elementos del subárbol izquierdo son menores y los del derecho son mayores.

### 1.1 Búsqueda

#### Python
```python
class Nodo:
    def __init__(self, valor):
        self.valor = valor
        self.izq = None
        self.der = None

def buscar(nodo: Nodo, x: int) -> Nodo:
    if nodo is None:
        return None
    if x == nodo.valor:
        return nodo
    elif x < nodo.valor:
        return buscar(nodo.izq, x)
    else:
        return buscar(nodo.der, x)
```

#### Pseudocódigo
```text
FUNCION Buscar(nodo: NODO, x: ENTERO) -> NODO
    SI nodo = NULO ENTONCES
        RETORNAR NULO
    FIN SI
    SI x = nodo.valor ENTONCES
        RETORNAR nodo
    SINO SI x < nodo.valor ENTONCES
        RETORNAR Buscar(nodo.izq, x)
    SINO
        RETORNAR Buscar(nodo.der, x)
    FIN SI
FIN FUNCION
```

### 1.2 Inserción

#### Python
```python
def insertar(nodo: Nodo, x: int) -> Nodo:
    if nodo is None:
        return Nodo(x)
    if x < nodo.valor:
        nodo.izq = insertar(nodo.izq, x)
    elif x > nodo.valor:
        nodo.der = insertar(nodo.der, x)
    # Si x == nodo.valor, no hacemos nada (claves únicas)
    return nodo
```

#### Pseudocódigo
```text
FUNCION Insertar(nodo: NODO, x: ENTERO) -> NODO
    SI nodo = NULO ENTONCES
        RETORNAR NuevoNodo(x)
    FIN SI
    SI x < nodo.valor ENTONCES
        nodo.izq <- Insertar(nodo.izq, x)
    SINO SI x > nodo.valor ENTONCES
        nodo.der <- Insertar(nodo.der, x)
    FIN SI
    RETORNAR nodo
FIN FUNCION
```

### 1.3 Eliminación (Compleja)
Caso recursivo que maneja eliminación de hojas, nodos con 1 hijo y nodos con 2 hijos (usando el mínimo del subárbol derecho).

#### Python
```python
def buscar_min(nodo: Nodo) -> int:
    actual = nodo
    while actual.izq is not None:
        actual = actual.izq
    return actual.valor

def eliminar(nodo: Nodo, x: int) -> Nodo:
    if nodo is None:
        return None
    
    if x < nodo.valor:
        nodo.izq = eliminar(nodo.izq, x)
    elif x > nodo.valor:
        nodo.der = eliminar(nodo.der, x)
    else:
        # Nodo encontrado
        if nodo.izq is None: # 0 o 1 hijo (derecho)
            return nodo.der
        elif nodo.der is None: # 1 hijo (izquierdo)
            return nodo.izq
        else: # 2 hijos
            min_val = buscar_min(nodo.der)
            nodo.valor = min_val
            nodo.der = eliminar(nodo.der, min_val)
    return nodo
```

#### Pseudocódigo
```text
FUNCION Eliminar(nodo: NODO, x: ENTERO) -> NODO
    SI nodo = NULO ENTONCES RETORNAR NULO
    
    SI x < nodo.valor ENTONCES
        nodo.izq <- Eliminar(nodo.izq, x)
    SINO SI x > nodo.valor ENTONCES
        nodo.der <- Eliminar(nodo.der, x)
    SINO
        SI nodo.izq = NULO ENTONCES
            RETORNAR nodo.der
        SINO SI nodo.der = NULO ENTONCES
            RETORNAR nodo.izq
        SINO
            min_val <- BuscarMin(nodo.der)
            nodo.valor <- min_val
            nodo.der <- Eliminar(nodo.der, min_val)
        FIN SI
    FIN SI
    RETORNAR nodo
FIN FUNCION
```

### 1.4 Recorridos

#### Python
```python
def in_orden(nodo):
    if nodo:
        in_orden(nodo.izq)
        print(nodo.valor)
        in_orden(nodo.der)

def pre_orden(nodo):
    if nodo:
        print(nodo.valor)
        pre_orden(nodo.izq)
        pre_orden(nodo.der)

def post_orden(nodo):
    if nodo:
        post_orden(nodo.izq)
        post_orden(nodo.der)
        print(nodo.valor)
```

#### Pseudocódigo
```text
PROCEDIMIENTO InOrden(nodo: NODO)
    SI nodo <> NULO ENTONCES
        InOrden(nodo.izq)
        IMPRIMIR nodo.valor
        InOrden(nodo.der)
    FIN SI
FIN PROCEDIMIENTO
```

---

## 2. Montículos (Max-Heap)
[Tutorialazo_impresionante_dios_bendiga_al_buen_hombre_que_lo_hizo][https://www.youtube.com/watch?v=NRzjL_wbcvs]
Árbol binario completo donde cada nodo es mayor o igual que sus hijos. Se suele implementar sobre un vector (índices 1 a N).

### 2.1 Flotar y Hundir

#### Python
```python
def flotar(M: list, i: int):
    # M[0] se ignora o guarda tamaño
    while i > 1 and M[i // 2] < M[i]:
        M[i // 2], M[i] = M[i], M[i // 2]
        i = i // 2

def hundir(M: list, i: int, n: int):
    hijo_izq = 2 * i
    while hijo_izq <= n:
        mayor = hijo_izq
        if hijo_izq + 1 <= n and M[hijo_izq + 1] > M[hijo_izq]:
            mayor = hijo_izq + 1
        
        if M[i] < M[mayor]:
            M[i], M[mayor] = M[mayor], M[i]
            i = mayor
            hijo_izq = 2 * i
        else:
            break
```

#### Pseudocódigo
```text
PROCEDIMIENTO Flotar(M: ARREGLO, i: ENTERO)
    MIENTRAS i > 1 Y M[i DIV 2] < M[i] HACER
        Intercambiar(M[i], M[i DIV 2])
        i <- i DIV 2
    FIN MIENTRAS
FIN PROCEDIMIENTO

PROCEDIMIENTO Hundir(M: ARREGLO, i: ENTERO, n: ENTERO)
    REPETIR
        hijo_izq <- 2 * i
        hijo_der <- 2 * i + 1
        mayor <- i
        
        SI hijo_izq <= n Y M[hijo_izq] > M[mayor] ENTONCES
            mayor <- hijo_izq
        FIN SI
        SI hijo_der <= n Y M[hijo_der] > M[mayor] ENTONCES
            mayor <- hijo_der
        FIN SI
        
        SI mayor <> i ENTONCES
            Intercambiar(M[i], M[mayor])
            i <- mayor
        SINO
            SALIR
        FIN SI
    FIN REPETIR
FIN PROCEDIMIENTO
```

### 2.2 Insertar y Eliminar Máximo

#### Python
```python
def insertar_monticulo(M: list, x: int):
    M.append(x)
    n = len(M) - 1 # Asumiendo M[0] reservado
    flotar(M, n)

def eliminar_max(M: list) -> int:
    n = len(M) - 1
    if n < 1: return None
    maximo = M[1]
    M[1] = M[n]
    M.pop()
    hundir(M, 1, len(M) - 1)
    return maximo
```

#### Pseudocódigo
```text
FUNCION EliminarMax(M: ARREGLO, n: ENTERO) -> ENTERO
    maximo <- M[1]
    M[1] <- M[n]
    n <- n - 1
    Hundir(M, 1, n)
    RETORNAR maximo
FIN FUNCION
```

---

## 3. Conjuntos Disjuntos (Union-Find)

### 3.1 Implementación con Bosques (Optimizado)
Usa **unión por rango/altura** y **compresión de caminos**.

#### Python
```python
padre = [] # Inicializado tal que padre[i] = i
altura = [] # Inicializado a 1 o 0

def buscar_optimizado(i: int) -> int:
    if padre[i] == i:
        return i
    padre[i] = buscar_optimizado(padre[i]) # Compresión de camino
    return padre[i]

def unir_optimizado(i: int, j: int):
    raiz_i = buscar_optimizado(i)
    raiz_j = buscar_optimizado(j)
    
    if raiz_i != raiz_j:
        if altura[raiz_i] > altura[raiz_j]:
            padre[raiz_j] = raiz_i
        elif altura[raiz_j] > altura[raiz_i]:
            padre[raiz_i] = raiz_j
        else:
            padre[raiz_j] = raiz_i
            altura[raiz_i] += 1
```

#### Pseudocódigo
```text
FUNCION Buscar(i: ENTERO) -> ENTERO
    SI padre[i] = i ENTONCES
        RETORNAR i
    FIN SI
    padre[i] <- Buscar(padre[i])  // Compresión de caminos
    RETORNAR padre[i]
FIN FUNCION

PROCEDIMIENTO Unir(i: ENTERO, j: ENTERO)
    raiz_i <- Buscar(i)
    raiz_j <- Buscar(j)
    SI raiz_i <> raiz_j ENTONCES
        SI altura[raiz_i] > altura[raiz_j] ENTONCES
            padre[raiz_j] <- raiz_i
        SINO SI altura[raiz_j] > altura[raiz_i] ENTONCES
            padre[raiz_i] <- raiz_j
        SINO
            padre[raiz_j] <- raiz_i
            altura[raiz_i] <- altura[raiz_i] + 1
        FIN SI
    FIN SI
FIN PROCEDIMIENTO
```

---

## 4. Tablas de Dispersión (Hash)

### 4.1 Exploración Lineal (Dispersión Cerrada)
Manejo de colisiones buscando la siguiente celda libre secuencialmente.

#### Python
```python
# T es la tabla, N es su tamaño. -1 indica vacío.
def insertar_hash(T: list, clave: int, N: int) -> bool:
    idx = clave % N
    intentos = 0
    while T[idx] != -1 and intentos < N:
        idx = (idx + 1) % N
        intentos += 1
    
    if T[idx] == -1:
        T[idx] = clave
        return True
    return False # Tabla llena

def buscar_hash(T: list, clave: int, N: int) -> int:
    idx = clave % N
    intentos = 0
    while T[idx] != -1 and intentos < N:
        if T[idx] == clave:
            return idx
        idx = (idx + 1) % N
        intentos += 1
    return -1
```

#### Pseudocódigo
```text
FUNCION InsertarHash(T: ARREGLO, clave: ENTERO, N: ENTERO) -> BOOLEANO
    i <- clave MOD N
    intentos <- 0
    MIENTRAS T[i] <> VACIO Y intentos < N HACER
        i <- (i + 1) MOD N
        intentos <- intentos + 1
    FIN MIENTRAS
    
    SI T[i] = VACIO ENTONCES
        T[i] <- clave
        RETORNAR VERDADERO
    SINO
        RETORNAR FALSO
    FIN SI
FIN FUNCION
```
