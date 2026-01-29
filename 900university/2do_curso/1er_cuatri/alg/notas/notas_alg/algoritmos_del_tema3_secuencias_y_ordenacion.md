# Algoritmos del Tema 3: Secuencias y Conjuntos de Datos (Ordenación)

Esta nota detalla los algoritmos de ordenación estudiados, sus estrategias y complejidades.

---

## 1. Ordenación por Inserción

Estrategia iterativa que mantiene una sublista ordenada y "inserta" el siguiente elemento en su posición correcta.

### Complejidad
- **Peor caso**: $\Theta(n^2)$ (array inverso).
- **Mejor caso**: $\Theta(n)$ (array ya ordenado).
- **Caso medio**: $\Theta(n^2)$ (número de inversiones $I 
approx n^2/4$).

### Pseudocódigo
```text
PROCEDIMIENTO OrdenacionInsercion(VAR T[1..n]: ARREGLO)
    PARA i <- 2 HASTA n HACER
        x <- T[i]
        j <- i - 1
        MIENTRAS j > 0 Y T[j] > x HACER
            T[j+1] <- T[j]
            j <- j - 1
        FIN MIENTRAS
        T[j+1] <- x
    FIN PARA
FIN PROCEDIMIENTO
```

### Python
```python
def insertion_sort(arr):
    for i in range(1, len(arr)):
        key = arr[i]
        j = i - 1
        while j >= 0 and key < arr[j]:
            arr[j + 1] = arr[j]
            j -= 1
        arr[j + 1] = key
```

---

## 2. Ordenación Shell (ShellSort)

Mejora de la inserción permitiendo intercambios de elementos lejanos. Utiliza una secuencia de **incrementos** $h_k, h_{k-1}, …, 1$.
El array se ordena "h-ordenándolo" (elementos a distancia $h$ están ordenados entre sí).

### Incrementos
- **Shell**: $h_t = ⌈n/2⌉, h_k = ⌈h_{k+1}/2⌉$. Peor caso $\Theta(n^2)$.
- **Hibbard**: $2^k - 1$. Peor caso $\Theta(n^{3/2})$.
- **Sedgewick**: $4^k + 3\cdot 2^{k-1} + 1$. Peor caso $\Theta(n^{4/3})$.

### Pseudocódigo
```text
PROCEDIMIENTO OrdenacionShell(VAR T[1..n]: ARREGLO)
    incremento <- n
    REPETIR
        incremento <- incremento DIV 2
        PARA i <- incremento + 1 HASTA n HACER
            tmp <- T[i]
            j <- i
            seguir <- CIERTO
            MIENTRAS j > incremento Y seguir HACER
                SI tmp < T[j - incremento] ENTONCES
                    T[j] <- T[j - incremento]
                    j <- j - incremento
                SINO
                    seguir <- FALSO
                FIN SI
            FIN MIENTRAS
            T[j] <- tmp
        FIN PARA
    HASTA incremento = 1
FIN PROCEDIMIENTO
```

### Python
```python
def shell_sort(arr):
    n = len(arr)
    gap = n // 2
    while gap > 0:
        for i in range(gap, n):
            temp = arr[i]
            j = i
            while j >= gap and arr[j - gap] > temp:
                arr[j] = arr[j - gap]
                j -= gap
            arr[j] = temp
        gap //= 2
```

---

## 3. Ordenación por Montículos (Heapsort)

Utiliza un **Max-Heap** (Montículo de Máximos).
1. Construye el montículo inicial: $\Theta(n)$.
2. Extrae el máximo repetidamente y lo coloca al final: $n × \Theta(\log n)$.

### Complejidad
- **Siempre**: $\Theta(n × \log n)$.
- **Espacio**: $\Theta(1)$ (in-place).

### Pseudocódigo
```text
// Asume procedimientos Flotar y Hundir del Tema 2
PROCEDIMIENTO Heapsort(VAR T[1..n]: ARREGLO)
    // 1. Crear Montículo
    PARA i <- n DIV 2 HASTA 1 PASO -1 HACER
        Hundir(T, i, n)
    FIN PARA
    
    // 2. Ordenar
    PARA i <- n HASTA 2 PASO -1 HACER
        Intercambiar(T[1], T[i]) // Mover máximo al final
        Hundir(T, 1, i - 1)      // Restaurar propiedad heap
    FIN PARA
FIN PROCEDIMIENTO
```

### Python
```python
def heapify(arr, n, i):
    largest = i
    l = 2 * i + 1
    r = 2 * i + 2
    if l < n and arr[l] > arr[largest]:
        largest = l
    if r < n and arr[r] > arr[largest]:
        largest = r
    if largest != i:
        arr[i], arr[largest] = arr[largest], arr[i]
        heapify(arr, n, largest)

def heap_sort(arr):
    n = len(arr)
    # Construir max-heap
    for i in range(n // 2 - 1, -1, -1):
        heapify(arr, n, i)
    # Extraer elementos
    for i in range(n - 1, 0, -1):
        arr[i], arr[0] = arr[0], arr[i]
        heapify(arr, i, 0)
```

---

## 4. Ordenación por Fusión (MergeSort)

Estrategia **Divide y Vencerás**.
1. Divide el array en dos mitades.
2. Ordena recursivamente cada mitad.
3. **Fusiona** las dos mitades ordenadas.

### Complejidad
- **Siempre**: $\Theta(n × \log n)$.
- **Espacio**: $\Theta(n)$ (requiere array auxiliar para la fusión).

### Pseudocódigo (Fusión)
```text
PROCEDIMIENTO Fusion(T, Izda, Centro, Dcha)
    // Copiar a Auxiliar...
    i <- Izda; j <- Centro + 1; k <- Izda
    MIENTRAS i <= Centro Y j <= Dcha HACER
        SI T[i] <= T[j] ENTONCES
            Aux[k] <- T[i]; i <- i + 1
        SINO
            Aux[k] <- T[j]; j <- j + 1
        FIN SI
        k <- k + 1
    FIN MIENTRAS
    // Copiar restantes...
FIN PROCEDIMIENTO
```

### Python
```python
def merge_sort(arr):
    if len(arr) > 1:
        mid = len(arr) // 2
        L = arr[:mid]
        R = arr[mid:]

        merge_sort(L)
        merge_sort(R)

        i = j = k = 0
        while i < len(L) and j < len(R):
            if L[i] <= R[j]:
                arr[k] = L[i]
                i += 1
            else:
                arr[k] = R[j]
                j += 1
            k += 1

        while i < len(L):
            arr[k] = L[i]
            i += 1
            k += 1
        while j < len(R):
            arr[k] = R[j]
            j += 1
            k += 1
```

---

## 5. Ordenación Rápida (QuickSort)

Estrategia **Divide y Vencerás**.
1. Elige un **pivote**.
2. **Partición**: Reorganiza el array tal que los menores que el pivote estén a la izquierda y los mayores a la derecha.
3. Ordena recursivamente las sublistas.

### Selección de Pivote
- **Primer elemento**: Malo si la entrada está casi ordenada ($O(n^2)$).
- **Aleatorio**: Minimiza probabilidad de peor caso.
- **Mediana de 3**: (Primero, Central, Último). Usado habitualmente.

### Complejidad
- **Peor caso**: $\Theta(n^2)$ (desbalanceo total).
- **Caso medio**: $\Theta(n × \log n)$.
- **Espacio**: $\Theta(\log n)$ (pila de recursión).

### Pseudocódigo
```text
PROCEDIMIENTO QuickSort(VAR T, i, j)
    SI i < j ENTONCES
        pivote <- Particion(T, i, j)
        QuickSort(T, i, pivote - 1)
        QuickSort(T, pivote + 1, j)
    FIN SI
FIN PROCEDIMIENTO

FUNCION Particion(VAR T, i, j) -> ENTERO
    pivote <- T[i] // Ejemplo simple
    izq <- i + 1
    der <- j
    REPETIR
        MIENTRAS T[izq] <= pivote Y izq <= der HACER izq <- izq + 1
        MIENTRAS T[der] > pivote Y der >= izq HACER der <- der - 1
        SI izq < der ENTONCES Intercambiar(T[izq], T[der])
    HASTA izq > der
    Intercambiar(T[i], T[der])
    RETORNAR der
FIN FUNCION
```

### Python (Mediana de 3 Simplificada)
```python
def partition(arr, low, high):
    # Selección pivote (ej. último elemento para simplicidad aquí)
    pivot = arr[high] 
    i = low - 1
    for j in range(low, high):
        if arr[j] <= pivot:
            i += 1
            arr[i], arr[j] = arr[j], arr[i]
    arr[i + 1], arr[high] = arr[high], arr[i + 1]
    return i + 1

def quick_sort(arr, low, high):
    if low < high:
        pi = partition(arr, low, high)
        quick_sort(arr, low, pi - 1)
        quick_sort(arr, pi + 1, high)
```

```