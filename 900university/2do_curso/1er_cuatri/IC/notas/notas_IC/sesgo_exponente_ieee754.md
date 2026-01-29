# IEEE 754: El Sesgo (Bias) del Exponente y Patrones Especiales

En el formato de punto flotante IEEE 754, el exponente debe ser capaz de representar tanto valores positivos como negativos (para números muy grandes o muy pequeños). Sin embargo, el campo de bits para el exponente se interpreta como un número sin signo. Para resolver esta discrepancia, se utiliza un "sesgo" (bias).

## Fórmula del Sesgo

El valor del sesgo se calcula con la fórmula: `Bias = 2^(k-1) - 1`, donde `k` es el número de bits asignados al campo del exponente.

*   **Para Precisión Simple (32 bits):** `k = 8` bits.
    `Bias = 2^(8-1) - 1 = 2^7 - 1 = 128 - 1 = 127`.
*   **Para Precisión Doble (64 bits):** `k = 11` bits.
    `Bias = 2^(11-1) - 1 = 2^10 - 1 = 1024 - 1 = 1023`.

## ¿Por qué el "-1" en la Fórmula del Sesgo?

La fórmula `2^(k-1) - 1` para el sesgo surge de la necesidad de reservar dos patrones de bits específicos en el campo del exponente para manejar situaciones especiales:

1.  **Exponente Almacenado de `0` (todos los bits a cero, ej: `00000000`):**
    *   Este patrón se utiliza para representar el número **cero** y los **números denormalizados** (números muy pequeños, cercanos a cero, que no pueden ser representados con la normalización estándar).
2.  **Exponente Almacenado de `2^k - 1` (todos los bits a uno, ej: `11111111`):**
    *   Este patrón se utiliza para representar **infinito** (`+∞` o `-∞`, dependiendo del bit de signo) y **NaN** (Not a Number), que indica resultados de operaciones indefinidas (como `0/0`).

Debido a la reserva de estos dos patrones extremos, el rango de valores que el exponente almacenado puede tomar para representar **números normalizados** es `[1, (2^k - 2)]`.

Para la precisión simple (`k=8`), el rango de exponentes almacenados útiles es de `1` a `254`. El sesgo de `127` permite que este rango se traduzca en exponentes reales que van desde `-126` (`1 - 127`) hasta `+127` (`254 - 127`), logrando una distribución casi simétrica alrededor de cero.

## Exponente Real vs. Sesgo

Es crucial diferenciar estos dos conceptos:

*   El **exponente real** (`n` o `E`) es la potencia de 2 en la representación binaria normalizada del número (`1.M * 2^E`). Este `E` puede ser positivo o negativo, indicando la magnitud del número.
*   El **sesgo (bias)** es un valor constante que se suma al exponente real (`Exponente_Almacenado = Exponente_Real + Bias`) para que el valor resultante (`Exponente_Almacenado`) siempre sea positivo o cero, permitiendo su almacenamiento en un campo de bits sin signo.

El sesgo es, por tanto, un desplazamiento del exponente real, no el exponente en sí mismo. Su objetivo es facilitar el almacenamiento y la comparación de números de punto flotante.
