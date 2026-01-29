# Algoritmos del Tema 5: Diseño por Inducción y Programación Dinámica

Este tema se centra en la construcción de soluciones a partir de subproblemas, ya sea de forma descendente (Divide y Vencerás) o ascendente (Programación Dinámica).

---

## 1. Divide y Vencerás (D&V)

Estrategia descendente que divide el problema en subproblemas independientes, los resuelve recursivamente y combina los resultados.

### Esquema General
```text
FUNCION DivideYVenceras(x)
    SI x es pequeño ENTONCES
        RETORNAR SolucionDirecta(x)
    SINO
        Descomponer x en x1, x2, ..., xa
        PARA i <- 1 HASTA a HACER
            yi <- DivideYVenceras(xi)
        FIN PARA
        RETORNAR Combinar(y1, y2, ..., ya)
    FIN SI
FIN FUNCION
```

---

## 2. Programación Dinámica (PD)

Estrategia ascendente que resuelve cada subproblema una sola vez y guarda el resultado en una tabla para evitar cálculos redundantes. Se basa en el **Principio de Optimalidad**.

### 2.1 Sucesión de Fibonacci (Optimización PD)

#### Python (Ascendente - Espacio O(1))
```python
def fibonacci_pd(n):
    if n < 2: return n
    prev2, prev1 = 0, 1
    for i in range(2, n + 1):
        actual = prev1 + prev2
        prev2 = prev1
        prev1 = actual
    return prev1
```

#### Pseudocódigo
```text
FUNCION Fibonacci(n)
    i <- 1; j <- 0
    PARA k <- 1 HASTA n HACER
        j <- i + j
        i <- j - i
    FIN PARA
    RETORNAR j
FIN FUNCION
```

### 2.2 Coeficientes Binomiales

Basado en la relación: $\binom{n}{k} = \binom{n-1}{k-1} + \binom{n-1}{k}$.

#### Python
```python
def binomial_coeff(n, k):
    C = [[0 for _ in range(k + 1)] for _ in range(n + 1)]
    for i in range(n + 1):
        for j in range(min(i, k) + 1):
            if j == 0 or j == i:
                C[i][j] = 1
            else:
                C[i][j] = C[i-1][j-1] + C[i-1][j]
    return C[n][k]
```

---

### 2.3 Problema del Cambio de Monedas (Mínimas Monedas)

Dadas denominaciones $v_1, \dots, v_m$ y un importe $n$, minimizar el número de monedas.

#### Pseudocódigo
```text
FUNCION CambioPD(monedas[1..m], n)
    // Inicializar tabla C[1..m, 0..n]
    PARA i <- 1 HASTA m HACER C[i, 0] <- 0 FIN PARA
    
    PARA i <- 1 HASTA m HACER
        PARA j <- 1 HASTA n HACER
            SI i = 1 Y j < monedas[i] ENTONCES
                C[i, j] <- INFINITO
            SINO SI i = 1 ENTONCES
                C[i, j] <- 1 + C[1, j - monedas[1]]
            SINO SI j < monedas[i] ENTONCES
                C[i, j] <- C[i-1, j]
            SINO
                C[i, j] <- MIN(C[i-1, j], 1 + C[i, j - monedas[i]])
            FIN SI
        FIN PARA
    FIN PARA
    RETORNAR C[m, n]
FIN FUNCION
```

---

### 2.4 Problema de la Mochila 0/1 (No fraccionaria)

Maximizar valor sin fraccionar objetos. Tabla $M[i, j]$ representa el valor máximo con capacidad $j$ usando objetos hasta $i$.

#### Python (Construcción de Tabla)
```python
def knapsack_01(W, weights, values):
    n = len(weights)
    M = [[0 for _ in range(W + 1)] for _ in range(n + 1)]
    
    for i in range(1, n + 1):
        for j in range(1, W + 1):
            if weights[i-1] <= j:
                M[i][j] = max(M[i-1][j], M[i-1][j - weights[i-1]] + values[i-1])
            else:
                M[i][j] = M[i-1][j]
    return M
```

#### Reconstrucción de la solución (Función Composición)
```text
FUNCION Composicion(M[1..n, 0..W], w[1..n]) -> C[1..n]
    PARA i <- 1 HASTA n HACER C[i] <- FALSO FIN PARA
    j <- W
    PARA i <- n HASTA 2 PASO -1 HACER
        SI M[i, j] <> M[i-1, j] ENTONCES
            C[i] <- VERDADERO
            j <- j - w[i]
        FIN SI
    FIN PARA
    SI j > 0 ENTONCES C[1] <- VERDADERO FIN SI
    RETORNAR C
FIN FUNCION
```
