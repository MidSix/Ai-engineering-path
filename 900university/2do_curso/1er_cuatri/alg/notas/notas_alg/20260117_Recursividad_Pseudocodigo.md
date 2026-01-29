---
id: 20260117_Recursividad_Pseudocodigo
title: Recursividad en Pseudocódigo vs Python
tags:
  - algoritmos
  - recursividad
  - pseudocodigo
  - python
links:
  - [[T1]]
  - [[20260117_Pseudocodigo_desde_Python]]
---

# Recursividad: De la implementación al concepto

La recursividad se mantiene muy fiel entre Python y el pseudocódigo, ya que ambos se basan en la idea de una función que se llama a sí misma. Sin embargo, en pseudocódigo somos más explícitos con la estructura lógica para facilitar el análisis de costes (como el Teorema Maestro).

## 1. La Estructura Fundamental

Todo algoritmo recursivo debe tener:
1.  **Caso Base:** La condición de parada (evita el bucle infinito).
2.  **Caso Recursivo:** La llamada a la función con un subproblema más pequeño.

| Componente | Python | Pseudocódigo |
| :--- | :--- | :--- |
| Definición | `def f(n):` | `procedimiento f(n)` |
| Caso Base | `if n == 0: return 1` | `si n = 0 entonces devolver 1` |
| Llamada | `return n * f(n-1)` | `devolver n * f(n-1)` |

## 2. Ejemplo: Factorial

Este es el ejemplo más sencillo para entender la traducción directa.

**Python:**
```python
def factorial(n):
    if n <= 1:
        return 1
    else:
        return n * factorial(n - 1)
```

**Pseudocódigo:**
```text
procedimiento FACTORIAL(n)
    si n <= 1 entonces
        devolver 1
    si no
        devolver n * FACTORIAL(n - 1)
    fin si
fin procedimiento
```

## 3. Ejemplo Complejo: Búsqueda Binaria

Aquí es donde el pseudocódigo ayuda a visualizar la división del problema ($n/2$).

**Python:**
```python
def busqueda_binaria(A, bajo, alto, x):
    if bajo > alto:
        return -1
    
    medio = (bajo + alto) // 2
    
    if A[medio] == x:
        return medio
    elif A[medio] > x:
        return busqueda_binaria(A, bajo, medio - 1, x)
    else:
        return busqueda_binaria(A, medio + 1, alto, x)
```

**Pseudocódigo:**
```text
procedimiento BUSQUEDA-BINARIA(A, bajo, alto, x)
    si bajo > alto entonces
        devolver -1
    fin si
    
    medio <- (bajo + alto) / 2  // División entera
    
    si A[medio] = x entonces
        devolver medio
    si no, si A[medio] > x entonces
        devolver BUSQUEDA-BINARIA(A, bajo, medio - 1, x)
    si no
        devolver BUSQUEDA-BINARIA(A, medio + 1, alto, x)
    fin si
fin procedimiento
```

## 4. Diferencias en la Notación de Retorno

En Python, `return` finaliza la ejecución de la función. En pseudocódigo, a veces verás `devolver` o simplemente asignar el resultado al nombre de la función (estilo Pascal), pero en esta asignatura usaremos `devolver` por claridad.

> **Importante para el análisis:** En el pseudocódigo recursivo es donde mejor identificamos los parámetros para el **Teorema Maestro**:
> - El número de llamadas recursivas en el cuerpo es $a$.
> - El cambio en los parámetros de rango (`bajo`, `alto`) nos da $b$.

---

¿Deseas crear una nota sobre cómo pasar de **pseudocódigo recursivo a una ecuación de recurrencia** ($T(n) = ...$) para aplicar el Teorema Maestro?
