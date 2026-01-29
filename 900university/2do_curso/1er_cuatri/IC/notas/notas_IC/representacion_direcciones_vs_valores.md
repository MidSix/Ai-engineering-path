# Representación Numérica: Direcciones de Memoria vs. Valores con Signo

La elección del sistema de representación binaria no es arbitraria; depende fundamentalmente de la naturaleza de los datos que se desean representar. Existe una distinción clave entre cómo se representan las direcciones de memoria y cómo se representan los valores de datos que pueden ser negativos.

## 1. Direcciones y Magnitudes (Binario sin Signo)

Este enfoque se aplica a cantidades que son inherentemente no negativas.

*   **¿Qué representan?:** Direcciones de memoria, índices de caché (líneas, conjuntos), números de página, desplazamientos (offsets), tamaños o contadores. No tiene sentido físico hablar de una "dirección de memoria -100" o una "página -3".
*   **Sistema Utilizado:** **Binario Puro** o **Binario sin Signo** (Unsigned).
*   **Funcionamiento:** Cada patrón de bits se utiliza para representar un número positivo o cero. Con `N` bits, se puede direccionar un total de `2^N` elementos distintos.
*   **Rango:** de `0` a `2^N - 1`.
*   **Ejemplo:** Para direccionar `2^6 = 64` líneas de una caché, se necesitan 6 bits, que cubren el rango de `0` (`000000`) a `63` (`111111`).

## 2. Valores de Datos (Sistemas con Signo)

Este enfoque se aplica a datos que pueden tomar valores tanto positivos como negativos.

*   **¿Qué representan?:** Resultados de operaciones aritméticas, datos de sensores (como la temperatura), etc.
*   **Sistemas Utilizados:** [[sistemas_representacion_enteros_signo|Signo-Magnitud, Complemento a 1 (CA1) y, principalmente, Complemento a 2 (CA2)]].
*   **Funcionamiento:** Se reserva un bit (el MSB) para indicar el signo. Esto significa que una parte del rango total de combinaciones de bits se "sacrifica" para poder representar números negativos.
*   **Rango (Ejemplo CA2):** de `-(2^(N-1))` a `+(2^(N-1) - 1)`.
*   **Ejemplo:** Con 6 bits en CA2, el rango es de `-32` a `+31`. Se pueden representar 64 números en total, pero casi la mitad son negativos.

---

## Notas Relacionadas

*   [[sistemas_representacion_enteros_signo]]
*   [[calculo_campos_direccion_cache]]
*   [[sesgo_exponente_ieee754]]
*   [[paginacion_multinivel]]
