# T1: Dualidad en Programación Lineal

A cada problema de programación lineal (**Primal**) le corresponde otro problema asociado llamado **Dual**. Ambos comparten la misma información pero desde perspectivas distintas (recursos vs. valoración).

## 1. Construcción del Problema Dual (Tabla de Conversión)

Si el Primal es de **Maximización**, su Dual será de **Minimización** (y viceversa).

| Primal (Max) | Dual (Min) |
| :--- | :--- |
| Matriz de coeficientes ($A$) | Matriz traspuesta ($A^T$) |
| Vector de recursos ($b$) | Coeficientes de la F.O. |
| Coeficientes de la F.O. ($c$) | Vector de recursos |
| Restricción $i$-ésima $\le$ | Variable $y_i \ge 0$ |
| Restricción $i$-ésima $\ge$ | Variable $y_i \le 0$ |
| Restricción $i$-ésima $=$ | Variable $y_i$ libre |
| Variable $x_j \ge 0$ | Restricción $j$-ésima $\ge$ |
| Variable $x_j$ libre | Restricción $j$-ésima $=$ |

## 2. Teoremas Fundamentales

1.  **Teorema Débil:** Para cualquier solución factible del primal ($z$) y del dual ($w$), se cumple que $z \le w$.
    *   *Consecuencia:* Si el primal es no acotado, el dual es infactible.
2.  **Teorema Fundamental (Dualidad Fuerte):** Si el Primal tiene una solución óptima $x^*$, el Dual también tiene una solución óptima $y^*$ y se cumple que **$z^* = w^*$**.
3.  **Teorema de Holguras Complementarias:** En el óptimo, el producto de una variable por su restricción dual asociada debe ser cero.
    *   Si una variable es positiva ($x_j > 0$), su restricción dual asociada se cumple como igualdad (holgura dual = 0).
    *   Si una restricción tiene holgura ($s_i > 0$), su variable dual asociada debe ser cero ($y_i = 0$).

## 3. Interpretación Económica: Precios Sombra

Las variables duales ($y_i$) representan los **Precios Sombra**.
*   **Definición:** Es el incremento en el valor de la función objetivo ante un aumento unitario en la disponibilidad del recurso $i$.
*   Si un recurso no se agota (sobra), su valor marginal (precio sombra) es 0.

## 4. Obtención del Dual desde la Tabla Símplex Final

En la tabla óptima del Primal (Max), los valores del Dual se encuentran en la fila de los beneficios relativos ($\bar{c}_j$) bajo las columnas de la **identidad inicial** (holguras):
*   $y_i = - (\bar{c}$ de la variable de holgura asociada a la restricción $i$).
*   En general: $y = c_B \cdot B^{-1}$.
