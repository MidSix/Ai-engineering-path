# Práctica 4.3: Ejercicio 3 - Análisis Inverso

Este ejercicio es un "problema inverso" donde se deducen los tamaños de los espacios de memoria a partir de la configuración de la tabla de páginas.

### Datos del Ejercicio
*   **Tamaño de página:** 4 KiB.
*   **Tamaño de entrada de tabla de páginas (PTE):** 4 bytes (32 bits).
*   **Contenido de la PTE:** 1 bit de residencia + 14 bits de control + Número de Marco Físico (NMF), (O numero de pagina fisica, es lo mismo).

---
### a) ¿Cuál es el tamaño de la memoria física?

El tamaño de la memoria física viene determinado por el número de bits disponibles para el campo NMF en cada entrada de la tabla.

1.  **Calcular los bits del NMF:**
    *   El tamaño total de la PTE es de 32 bits.
    *   Bits para el NMF = `Tamaño PTE - Bits Residencia - Bits Control`
    *   Bits para el NMF = `32 - 1 - 14 = 17` bits.

2.  **Calcular el número de marcos físicos:**
    *   Con 17 bits, podemos direccionar `2^17` marcos físicos distintos, es decir, 131,072 marcos.

3.  **Calcular el tamaño total de la memoria física:**
    *   Tamaño Memoria Física = `Nº de Marcos * Tamaño de Página`
    *   Tamaño = `2^17 * 4 KiB = 2^17 * 2^12 bytes = 2^29 bytes`.

4.  **Convertir a un formato legible:**
    *   `2^29 B = 2^9 * 2^20 B = 512 MiB`.

**Resultado (a):** El tamaño de la memoria física es de **512 MiB**.

---
### b) Si la traducción es de 2 niveles y cada tabla ocupa 1 página, ¿cuál es el tamaño del espacio virtual?

El tamaño del espacio virtual se deduce del número total de bits de la dirección virtual, que en un esquema de 2 niveles se compone de `[P1 | P2 | Desplazamiento]`.

1.  **Calcular bits de Desplazamiento:**
    *   Depende del tamaño de página: `log₂(4 KiB) = 12` bits.

2.  **Calcular bits de P2 (Índice de Nivel 2):**
    *   La condición es que "cada tabla de páginas ocupa exactamente 1 página".
    *   Número de entradas (PTEs) que caben en una tabla = `Tamaño de Página / Tamaño de PTE = 4 KiB / 4 B = 1024` entradas.
    *   Para direccionar estas 1024 entradas, se necesitan `log₂(1024) = 10` bits.
    *   Por tanto, **P2 = 10 bits**.

3.  **Calcular bits de P1 (Índice de Nivel 1):**
    *   La tabla de nivel 1 también ocupa 1 página, por lo que también tiene 1024 entradas.
    *   Para direccionar estas 1024 entradas, se necesitan `log₂(1024) = 10` bits.
    *   Por tanto, **P1 = 10 bits**.

4.  **Calcular bits totales de la dirección virtual:**
    *   Bits Totales = `P1 + P2 + Desplazamiento = 10 + 10 + 12 = 32` bits.

5.  **Calcular el tamaño del espacio virtual:**
    *   Tamaño EV = `2^32 bytes = 4 GiB`.

**Resultado (b):** El tamaño del espacio virtual es de **4 GiB**.

---
### c) Con el espacio virtual anterior, ¿cuánto ocuparía la tabla de páginas si la traducción se hiciese en 1 nivel?

Calculamos el tamaño de una única tabla de páginas para el espacio virtual de 4 GiB.

1.  **Calcular el número de páginas virtuales (y, por tanto, de entradas):**
    *   Nº de Páginas Virtuales = `Tamaño EV / Tamaño de Página = 2^32 / 2^12 = 2^20` entradas.

2.  **Calcular el tamaño total de la tabla:**
    *   Tamaño Tabla = `Nº de Entradas * Tamaño de PTE`
    *   Tamaño = `2^20 * 4 bytes = 4,194,304 bytes`.

3.  **Convertir a un formato legible:**
    *   `4,194,304 B / (1024*1024) = 4 MiB`.

**Resultado (c):** Con un solo nivel, la tabla de páginas ocuparía **4 MiB**, lo que demuestra la ventaja de espacio de la paginación de 2 niveles para procesos que no utilizan todo su espacio virtual.
