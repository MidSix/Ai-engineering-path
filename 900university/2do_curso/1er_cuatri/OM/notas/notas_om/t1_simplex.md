# T1: Programación Lineal y el Método Símplex

Este tema sienta las bases de la resolución algebraica de problemas de optimización lineal.

## 1. Conceptos Fundamentales

*   **Problema de Programación Lineal:** Buscar el máximo o mínimo de una función lineal sujeta a restricciones lineales (igualdades o desigualdades).
*   **Conjunto Factible ($S$):** El poliedro formado por todas las soluciones que cumplen las restricciones.
*   **Solución Básica Factible (SBF):** Un punto extremo del poliedro. El Símplex se mueve entre estos puntos.
*   **Teorema Fundamental:** Si existe una solución óptima, al menos una SBF es óptima.

## 2. La Forma Estándar
Para aplicar el Símplex, el problema debe estar en forma estándar:
1.  **Objetivo:** Maximizar (si es Min, maximizar $-f$).
2.  **Restricciones:** Todas deben ser **igualdades** ($Ax = b$).
    *   Si es $\le$, sumar variable de holgura ($s_i$).
    *   Si es $\ge$, restar variable de exceso ($e_i$) y sumar variable artificial ($a_i$).
3.  **Lado derecho ($b$):** Siempre positivo ($b \ge 0$).
4.  **Variables:** Todas no negativas ($x_i \ge 0$).

## 3. El Algoritmo del Símplex (Paso a Paso)

### Paso 1: Inicialización
Obtener una base inicial (matriz identidad en la tabla). Si no es evidente, usar **Variables Artificiales**.

### Paso 2: Cálculo de Beneficios Relativos ($\bar{c}_j$)
Para cada variable no básica, calcular cuánto mejoraría el objetivo si entra en la base.
*   $\bar{c}_j = c_j - z_j$ (donde $z_j = c_B \cdot B^{-1} A_j$).
*   **Criterio de Parada:** Si todos los $\bar{c}_j \le 0$, la solución actual es **Óptima**.

### Paso 3: Selección de Variable Entrante (Columna Pivote)
Elegir la variable con el **mayor beneficio relativo positivo** ($\bar{c}_j > 0$).

### Paso 4: Selección de Variable Saliente (Fila Pivote)
Aplicar la **Regla de la Mínima Proporción**:
Para la columna elegida, dividir el lado derecho ($b_i$) entre los elementos positivos de la columna ($a_{ij} > 0$).
*   La fila con el menor cociente es la que sale de la base.
*   Si no hay elementos positivos en la columna, el problema es **No Acotado**.

### Paso 5: Pivotaje (Actualización de la Tabla)
Realizar operaciones elementales por fila (Gauss-Jordan) para que el elemento pivote sea 1 y el resto de su columna sea 0. Volver al Paso 2.

## 4. Casos Especiales en la Tabla Final
*   **Solución Única:** Todos los $\bar{c}_j$ de variables no básicas son estrictamente negativos ($< 0$).
*   **Infinitas Soluciones:** Un $\bar{c}_j$ de una variable no básica es $0$ (se puede mover a otro óptimo).
*   **Infactibilidad:** El algoritmo termina pero hay variables artificiales positivas en la base.
*   **Degeneración:** Un $b_i$ en la base es $0$. Puede causar bucles infinitos (ciclado).

## 5. Método de las Dos Fases (Para variables artificiales)
1.  **Fase I:** Minimizar la suma de las variables artificiales.
    *   Si el óptimo es $0$, pasar a la Fase II.
    *   Si es $> 0$, el problema original es infactible.
2.  **Fase II:** Eliminar columnas artificiales y resolver el problema original usando la base de la Fase I.
