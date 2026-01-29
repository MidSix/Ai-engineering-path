---
id: 20260117_Teorema_Maestro_Recurrencias
title: Relación entre Recurrencias, Teorema Maestro y Notación Asintótica
tags:
  - algoritmos
  - complejidad
  - recurrencia
  - teorema_maestro
  - divide_y_venceras
links:
  - [[T1]]
---


# El Puente: De la Recurrencia a la Notación Asintótica

Este concepto es fundamental para entender cómo analizamos algoritmos recursivos. A menudo confundimos la **ecuación** (que describe el proceso) con la **notación** (que clasifica el resultado).

## 1. El Punto de Partida: La Ecuación de Recurrencia ($=$)

Cuando escribimos un algoritmo recursivo (Divide y Vencerás), no sabemos cuánto tarda en total. Solo sabemos describir su estructura paso a paso. Usamos una igualdad matemática:

$$T(n) = a \cdot T(n/b) + f(n)$$

*   **$a$ (o $\ell$):** Número de llamadas recursivas (ramas).
*   **$b$:** Factor de reducción del problema.
*   **$f(n)$:** Coste del trabajo local (fuera de la recursión), generalmente de la forma $c \cdot n^k$.

> **Analogía:** Esto es como decir "Mi sueldo anual es igual a 2 veces mi sueldo semestral más un bonus". Es una descripción exacta, pero no te dice cuánto ganas en total.

## 2. El Destino: La Notación Asintótica ($\leq$)

El objetivo es clasificar el algoritmo en una "clase de complejidad" (Big-O, Big-Theta). Esto ya no es una descripción mecánica, es una cota superior simplificada.

$$T(n) \in \Theta(g(n)) \implies T(n) \leq c \cdot g(n)$$

> **Analogía:** "Gano aproximadamente una cifra de 5 ceros". Es una simplificación útil para comparar.

---

## 3. El "Puente": El Árbol de Recursión

¿Cómo pasamos de la igualdad ($=$) a la clase de complejidad ($O$)? **Sumando el trabajo de todos los niveles del árbol de ejecución.**

El Teorema Maestro no es magia; es el resultado pre-calculado de sumar una serie geométrica que representa el árbol.

Imagina el coste en cada nivel del árbol:
1.  **Nivel 0 (Raíz):** Hacemos $n^k$ trabajo.
2.  **Nivel 1:** Tenemos $a$ subproblemas, cada uno de tamaño $n/b$. Trabajo total: $a \cdot (n/b)^k$.
3.  **Nivel Profundo:** Tenemos muchísimas hojas (casos base).

El Teorema Maestro compara la velocidad a la que crece el número de nodos ($a$) contra la velocidad a la que se reduce el coste por nodo ($b^k$).

### La Batalla de Fuerzas (Explicación Paso a Paso)

Para saber la complejidad final ($T(n)$), miramos quién domina la suma total del árbol:

#### Caso 1: La Recursión Domina ($a > b^k$)
*   **Qué pasa:** El árbol se ensancha muy rápido. Hay demasiadas hojas.
*   **Consecuencia:** El coste total es proporcional al número de hojas.
*   **De la Ecuación a la Asíntota:**
    *   Solución exacta: $T(n) \approx C_1 \cdot n^{\log_b a} - C_2 \dots$
    *   Simplificación Asintótica: **$T(n) = \Theta(n^{\log_b a})$**

#### Caso 2: Equilibrio ($a = b^k$)
*   **Qué pasa:** El trabajo es idéntico en cada nivel del árbol.
*   **Consecuencia:** El coste total es (Trabajo por nivel) $\times$ (Altura del árbol).
*   **De la Ecuación a la Asíntota:**
    *   Trabajo por nivel: $n^k$
    *   Altura árbol: $\log_b n$
    *   Simplificación Asintótica: **$T(n) = \Theta(n^k \log n)$**

#### Caso 3: El Trabajo Local Domina ($a < b^k$)
*   **Qué pasa:** Los subproblemas se encogen muy rápido. La raíz hace casi todo el trabajo.
*   **Consecuencia:** El coste total es proporcional al primer nivel (la raíz).
*   **De la Ecuación a la Asíntota:**
    *   Solución exacta: Serie geométrica decreciente que converge al primer término.
    *   Simplificación Asintótica: **$T(n) = \Theta(n^k)$**

---

## 4. Resumen Visual

| Concepto | Símbolo | Significado | Ejemplo (MergeSort) |
| :--- | :--- | :--- | :--- |
| **Recurrencia** | $T(n) = a T(n/b) + cn^k$ | Cómo funciona el código (mecánica). | $T(n) = 2T(n/2) + cn$ |
| **Parámetros** | $a=2, b=2, k=1$ | Las "fuerzas" en juego. | $2$ vs $2^1$ (Empate) |
| **Asíntota** | $T(n) \in \Theta(n \log n)$ | Cuánto tarda finalmente (clasificación). | Lineal-Logarítmico |

**Nota:** En tus diapositivas de clase, $a$ se denota como $\ell$. Es exactamente lo mismo.
