# T1: Análisis de Sensibilidad

Estudia cómo cambia la solución óptima si se modifican los datos originales del problema sin tener que volver a resolverlo desde cero.

## 1. Cambios en los Coeficientes del Objetivo ($c_j$)

### A. Para una variable no básica
*   **Acción:** Recalcular su beneficio relativo $\bar{c}_j$ con el nuevo valor.
*   **Resultado:**
    *   Si $\bar{c}_j \le 0$, la base actual **sigue siendo óptima**.
    *   Si $\bar{c}_j > 0$, la variable debe entrar en la base (seguir iterando con Símplex).

### B. Para una variable básica
*   Afecta a todos los beneficios relativos de las variables no básicas.
*   Se calcula el rango de valores para el cual la solución actual sigue siendo óptima resolviendo el sistema $\bar{c}_j \le 0$.

## 2. Cambios en los Recursos ($b_i$)

Modificar un recurso cambia el valor de la solución ($x_B = B^{-1}b$).
*   **Acción:** Actualizar el vector $b$ y calcular $x_B = B^{-1} \cdot b_{nuevo}$.
*   **Resultado:**
    *   Si todos los nuevos $x_B \ge 0$, la base **sigue siendo factible** (y el nuevo valor de $z = c_B \cdot x_B$).
    *   Si algún nuevo $x_B < 0$, la base es **infactible**. Se debe aplicar el **Símplex Dual** para recuperar la factibilidad.

## 3. Añadir una nueva Variable ($x_{n+1}$)

Es equivalente a estudiar una variable no básica que tenía $c_{n+1} = 0$.
1.  Calcular su beneficio relativo: $\bar{c}_{n+1} = c_{n+1} - c_B B^{-1} A_{n+1}$.
2.  Si $\bar{c}_{n+1} \le 0$, no es rentable producirla.
3.  Si $\bar{c}_{n+1} > 0$, entra en la base.

## 4. Añadir una nueva Restricción

1.  Comprobar si la solución óptima actual cumple la nueva restricción.
2.  Si la cumple, **no cambia nada**.
3.  Si NO la cumple, la solución actual deja de ser factible.
    *   Se añade la restricción a la tabla (usando variables de holgura).
    *   Se utiliza el **Símplex Dual** para encontrar el nuevo óptimo.

---

## El Símplex Dual (Resumen rápido)
Se usa cuando la tabla es **Óptima** (todos los $\bar{c}_j \le 0$) pero **Infactible** (algún $b_i < 0$).
*   **Sale de la base:** La variable con el $b_i$ más negativo.
*   **Entra en la base:** Se elige mediante la razón entre los beneficios relativos y los elementos negativos de la fila elegida ($\min \{ \bar{c}_j / a_{rj} \}$ para $a_{rj} < 0$).
