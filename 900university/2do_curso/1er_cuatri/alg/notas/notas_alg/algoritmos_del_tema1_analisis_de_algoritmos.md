# Algoritmos del Tema 1: Análisis de Algoritmos

Esta nota recopila los algoritmos estudiados en el Tema 1, incluyendo implementaciones en Python y su correspondiente pseudocódigo.

## 1. Ordenación por Inserción (Insertion Sort)
Algoritmo de ordenación estable que construye la lista ordenada elemento a elemento, insertando cada nuevo elemento en su posición correcta dentro de la parte ya ordenada.

### Python
```python
def insertion_sort(v: list[int]) -> None:
    n = len(v)
    for i in range(1, n):
        x = v[i]
        j = i - 1
        while j >= 0 and v[j] > x:
            v[j + 1] = v[j]
            j = j - 1
        v[j + 1] = x
```

### Pseudocódigo
```text
ALGORITMO InsertionSort(v: ARREGLO[0..n-1] DE ENTEROS)
    n <- LONGITUD(v)
    PARA i <- 1 HASTA n-1 HACER
        x <- v[i]
        j <- i - 1
        MIENTRAS j >= 0 Y v[j] > x HACER
            v[j+1] <- v[j]
            j <- j - 1
        FIN MIENTRAS
        v[j+1] <- x
    FIN PARA
FIN ALGORITMO
```

---

## 2. Ordenación por Selección (Selection Sort)
Algoritmo inestable que busca iterativamente el mínimo elemento de la sublista no ordenada y lo intercambia con el primer elemento de dicha sublista.

### Python
```python
def selection_sort(v: list[int]) -> None:
    n = len(v)
    for i in range(n - 1):
        min_j = i
        min_x = v[i]
        for j in range(i + 1, n):
            if v[j] < min_x:
                min_j = j
                min_x = v[j]
        v[min_j] = v[i]
        v[i] = min_x
```

### Pseudocódigo
```text
ALGORITMO SelectionSort(v: ARREGLO[0..n-1] DE ENTEROS)
    n <- LONGITUD(v)
    PARA i <- 0 HASTA n-2 HACER
        min_j <- i
        min_x <- v[i]
        PARA j <- i+1 HASTA n-1 HACER
            SI v[j] < min_x ENTONCES
                min_j <- j
                min_x <- v[j]
            FIN SI
        FIN PARA
        v[min_j] <- v[i]
        v[i] <- min_x
    FIN PARA
FIN ALGORITMO
```

---

## 3. Búsqueda Binaria (Binary Search)
Algoritmo eficiente para encontrar un elemento en una lista **ordenada**, dividiendo el espacio de búsqueda a la mitad en cada paso.

### Python
```python
def binary_search(x: int, v: list[int]) -> int:
    i, j = 0, len(v) - 1
    while i <= j:
        medio = (i + j) // 2
        if x < v[medio]:
            j = medio - 1
        elif x > v[medio]:
            i = medio + 1
        else:
            return medio
    return -1 # O None
```

### Pseudocódigo
```text
ALGORITMO BinarySearch(x: ENTERO, v: ARREGLO[0..n-1] DE ENTEROS) -> ENTERO
    i <- 0
    j <- LONGITUD(v) - 1
    MIENTRAS i <= j HACER
        medio <- (i + j) DIV 2
        SI x < v[medio] ENTONCES
            j <- medio - 1
        SINO SI x > v[medio] ENTONCES
            i <- medio + 1
        SINO
            RETORNAR medio
        FIN SI
    FIN MIENTRAS
    RETORNAR -1
FIN ALGORITMO
```

---

## 4. Potencia (Versión Iterativa)
Calcula $x^n$ mediante multiplicaciones sucesivas.

### Python
```python
def potencia_iterativa(x: float, n: int) -> float:
    res = 1.0
    for _ in range(n):
        res = res * x
    return res
```

### Pseudocódigo
```text
ALGORITMO PotenciaIterativa(x: REAL, n: ENTERO) -> REAL
    res <- 1.0
    PARA k <- 1 HASTA n HACER
        res <- res * x
    FIN PARA
    RETORNAR res
FIN ALGORITMO
```

---

## 5. Potencia (Versión Recursiva - Divide y Vencerás)
Calcula $x^n$ reduciendo el problema a la mitad del tamaño: $x^n = x^{n/2} \cdot x^{n/2}$.

### Python
```python
def potencia_recursiva(x: float, n: int) -> float:
    if n == 0:
        return 1.0
    if n % 2 == 0:
        temp = potencia_recursiva(x, n // 2)
        return temp * temp
    else:
        temp = potencia_recursiva(x, n // 2)
        return temp * temp * x
```

### Pseudocódigo
```text
ALGORITMO PotenciaRecursiva(x: REAL, n: ENTERO) -> REAL
    SI n = 0 ENTONCES
        RETORNAR 1.0
    FIN SI
    
    temp <- PotenciaRecursiva(x, n DIV 2)
    
    SI (n MOD 2) = 0 ENTONCES
        RETORNAR temp * temp
    SINO
        RETORNAR temp * temp * x
    FIN SI
FIN ALGORITMO
```

---

## 6. Fibonacci (Recursivo)
Ejemplo clásico de recursividad directa (ineficiente para $n$ grandes).

### Python
```python
def fibonacci_rec(n: int) -> int:
    if n <= 1:
        return n
    return fibonacci_rec(n - 1) + fibonacci_rec(n - 2)
```

### Pseudocódigo
```text
ALGORITMO FibonacciRec(n: ENTERO) -> ENTERO
    SI n <= 1 ENTONCES
        RETORNAR n
    FIN SI
    RETORNAR FibonacciRec(n-1) + FibonacciRec(n-2)
FIN ALGORITMO
```

---

## 7. Torres de Hanoi
Algoritmo recursivo para mover $n$ discos de un poste origen a un poste destino usando un auxiliar.

### Python
```python
def hanoi(n: int, origen: str, destino: str, auxiliar: str) -> None:
    if n == 1:
        print(f"Mover disco 1 de {origen} a {destino}")
        return
    
    hanoi(n - 1, origen, auxiliar, destino)
    print(f"Mover disco {n} de {origen} a {destino}")
    hanoi(n - 1, auxiliar, destino, origen)
```

### Pseudocódigo
```text
ALGORITMO Hanoi(n: ENTERO, origen: CADENA, destino: CADENA, auxiliar: CADENA)
    SI n = 1 ENTONCES
        IMPRIMIR "Mover disco 1 de ", origen, " a ", destino
        RETORNAR
    FIN SI
    
    Hanoi(n - 1, origen, auxiliar, destino)
    IMPRIMIR "Mover disco ", n, " de ", origen, " a ", destino
    Hanoi(n - 1, auxiliar, destino, origen)
FIN ALGORITMO
```
