---
id: 20260117_Pseudocodigo_desde_Python
title: Guía de Pseudocódigo para Programadores de Python
tags:
  - algoritmos
  - pseudocodigo
  - python
  - fundamentos
links:
  - [[T1]]
---

# De Python al Pseudocódigo: Guía de Traducción

El pseudocódigo es una herramienta para comunicar algoritmos sin preocuparse por la sintaxis específica de un lenguaje. En la asignatura de Algoritmos, se suele usar un estilo cercano al lenguaje natural o a lenguajes como Pascal/C, pero para un usuario de Python, la transición es muy directa.


## 1. Asignación y Variables

En Python usamos `=`, pero en pseudocódigo académico se prefiere la flecha `<-` para indicar que un valor se "mueve" hacia una variable.

| Operación   | Python        | Pseudocódigo         |
| :---------- | :------------ | :------------------- |
| Asignación  | `x = 10`      | `x := 10`            |
| Comparación | `if x == 10:` | `si x = 10 entonces` |

> **Nota:** En pseudocódigo, a veces un solo `=` se usa para comparar, por lo que la flecha `<-` ayuda a evitar ambigüedades en la asignación.

## 2. Estructuras de Control

### Condicionales
La estructura es prácticamente idéntica, pero añadimos palabras clave para delimitar bloques.

**Python:**
```python
if x > 0:
    print("Positivo")
elif x < 0:
    print("Negativo")
else:
    print("Cero")
```

**Pseudocódigo:**
```text
si x > 0 entonces
    escribir("Positivo")
si no, si x < 0 entonces
    escribir("Negativo")
si no
    escribir("Cero")
fin si
```

### Bucles (Mientas / While)
**Python:**
```python
while i < n:
    # cuerpo
    i += 1
```

**Pseudocódigo:**
```text
mientras i < n hacer
    # cuerpo
    i := i + 1
fin mientras
```

### Bucles (Para / For)
Python usa iteradores sobre rangos. El pseudocódigo suele especificar el rango de inicio y fin explícitamente.

**Python:**
```python
for i in range(1, n):
    # i va de 1 a n-1
```

**Pseudocódigo:**
```text
para i := 1 hasta n-1 hacer
    # cuerpo
fin para
```

## 3. Acceso a Datos (Listas y Arreglos)

En Python las listas empiezan en **0**. En muchos libros de algoritmos (y en los exámenes), los arreglos pueden empezar en **1**. ¡Ojo con esto!

| Concepto | Python | Pseudocódigo |
| :--- | :--- | :--- |
| Acceso | `A[i]` | `A[i]` |
| Longitud | `len(A)` | `A.longitud` o `n` |
| Intercambio | `A[i], A[j] = A[j], A[i]` | `intercambiar(A[i], A[j])` |

## 4. Definición de Funciones

**Python:**
```python
def mi_algoritmo(A, n):
    # código
    return x
```

**Pseudocódigo:**
```text
procedimiento mi_algoritmo(A, n)
    # código
    devolver x
fin procedimiento
```

---

## 5. Ejemplo Completo: Ordenación por Inserción

Vamos a ver cómo se traduce el algoritmo de `Insertion Sort`.

### Versión Python
```python
def insertion_sort(A):
    for j in range(1, len(A)):
        clave = A[j]
        i = j - 1
        while i >= 0 and A[i] > clave:
            A[i + 1] = A[i]
            i = i - 1
        A[i + 1] = clave
```

### Versión Pseudocódigo
```text
procedimiento INSERTION-SORT(A[1,...,n])
    para j := 2 hasta A.longitud hacer
        clave := A[j]
        // Insertar A[j] en la secuencia ordenada A[1..j-1]
        i := j - 1
        mientras i > 0 y A[i] > clave hacer
            A[i + 1] := A[i]
            i := i - 1
        fin mientras
        A[i + 1] := clave
    fin para
fin procedimiento
```

> **Diferencia clave:** En este ejemplo de pseudocódigo (estilo CLRS), el arreglo empieza en index **1**, por eso el bucle `para` empieza en **2** y el `mientras` comprueba `i > 0`.

---

## 6. Uso del punto y coma en pseudocódigo

## Idea clave
El pseudocódigo **no es un lenguaje formal**, por lo que el punto y coma (`;`) **no tiene significado semántico**.  
Su uso es **puramente estilístico** y no afecta al funcionamiento ni a la complejidad del algoritmo.

---
## Regla práctica segura

Si decides usar punto y coma en pseudocódigo:

> **Úsalo solo al final de instrucciones simples**  
> (asignaciones, llamadas a procedimientos, entrada/salida)

Y **no lo uses** en:
- encabezados de bucles
- encabezados de condicionales
- palabras estructurales
- cierres de bloques

---

## Instrucciones que pueden llevar `;`

Son **sentencias ejecutables simples**:

```text
x := T[i];
j := j - 1;
T[j+1] := x;
leer x;
escribir T[i];
