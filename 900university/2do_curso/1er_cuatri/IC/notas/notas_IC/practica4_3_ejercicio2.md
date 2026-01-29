# Práctica 4.3: Ejercicio 2 - Desglose

Este ejercicio explora la estructura de una tabla de páginas en un esquema de traducción directa (1 nivel) y el proceso de traducción de una dirección virtual a física.

### Datos del Ejercicio
*   **Esquema:** Traducción directa (1 nivel).
*   **Espacio Virtual (EV):** 64 GiB.
*   **Espacio Físico (EF):** 2 GiB.
*   **Tamaño de página:** 4 KiB.
*   **Entrada de Tabla de Páginas (PTE):** Contiene 1 bit de residencia, 12 bits de control y el número de marco físico (NMF).

---
### a) ¿Cuántas entradas tiene la tabla de páginas?

El número de entradas de la tabla corresponde al número total de páginas virtuales.

1.  **Tamaño del Espacio Virtual:** 64 GiB = 2^6 * 2^30 B = 2^36 bytes.
2.  **Tamaño de Página:** 4 KiB = 2^2 * 2^10 B = 2^12 bytes.
3.  **Número de Páginas Virtuales:** `Tamaño EV / Tamaño Página = 2^36 / 2^12 = 2^24`.

**Resultado:** La tabla de páginas tiene **2^24 entradas** (16,777,216 entradas).

---
### b) ¿Cuánto ocupa cada entrada en la tabla de páginas?

Para saber el tamaño total de la entrada (PTE), primero necesitamos saber cuántos bits ocupa el campo NMF.

1.  **Calcular bits del Número de Marco Físico (NMF):**
    *   **Tamaño del Espacio Físico:** 2 GiB = 2^1 * 2^30 B = 2^31 bytes.
    *   **Número de Marcos Físicos:** `Tamaño EF / Tamaño Página = 2^31 / 2^12 = 2^19`.
    *   Para numerar 2^19 marcos físicos, se necesitan **19 bits**.

2.  **Sumar los campos de la PTE:**
    *   Bit de Residencia (Válido): 1 bit
    *   Bits de Control: 12 bits (dado por el enunciado)
    *   Bits de NMF: 19 bits
    *   **Tamaño total de la PTE:** `1 + 12 + 19 = 32 bits`.

**Resultado:** Cada entrada ocupa **32 bits (4 bytes)**. Los "12 bits de control" son simplemente el espacio sobrante en la entrada de 32 bits una vez asignados los campos de Residencia y NMF.

---
### c) ¿Cuánto ocupa la tabla de páginas?

Multiplicamos el número total de entradas por el tamaño de cada una.

*   **Cálculo:** `Nº Entradas * Tamaño Entrada = 2^24 * 4 bytes = 67,108,864 bytes`.
*   **En MiB:** `67,108,864 / 2^20 = 64 MiB`.

**Resultado:** La tabla de páginas completa ocupa **64 MiB**.

---
### d) Describe el proceso de traducción de la dirección virtual `0x3A0042F64`

1.  **Descomponer la Dirección Virtual:**
    *   Una dirección virtual de 36 bits se divide en **NPV (24 bits)** y **Desplazamiento (12 bits)**.
    *   Dirección: `0x3A0042F64`.
    *   Los 12 bits menos significativos (3 dígitos hexadecimales) son el desplazamiento: **`0xF64`**.
    *   Los 24 bits más significativos (6 dígitos hexadecimales) son el NPV: **`0x3A0042`**.

2.  **Acceder a la Tabla de Páginas:**
    *   El sistema usa el NPV (`0x3A0042`) como índice para ir a la entrada correspondiente en la tabla de páginas.
    *   El enunciado nos dice que el contenido de esa entrada es `0x835F0340`.

3.  **Interpretar la Entrada de la Tabla (PTE):**
    *   El formato de la PTE de 32 bits es `[R(1) | Control(12) | NMF(19)]`.
    *   Contenido: `0x835F0340`.
    *   **Bit de Residencia (R):** Es el bit más significativo. `0x8...` en hexadecimal es `1000...` en binario. El bit es `1`, lo que significa que la página **está en memoria física**.
    *   **Número de Marco Físico (NMF):** Son los 19 bits menos significativos. `0x835F0340` en binario es `1000 0011 0101 1111 0000 0011 0100 0000`. Los 19 bits finales son `111 0000 0011 0100 0000`, que en hexadecimal es **`0x70340`**.

4.  **Construir la Dirección Física:**
    *   La dirección física se forma concatenando los bits del NMF con los del desplazamiento: `[NMF(19) | Desplazamiento(12)]`.
    *   Concatenamos `0x70340` y `0xF64`.
    *   `0x70340` en binario (19 bits) es `1110000001101000000`.
    *   `0xF64` en binario (12 bits) es `111101100100`.
    *   Juntos: `1110000001101000000` `111101100100`.
    *   Agrupando en hex: `0111 0000 0011 0100 0000` `1111 0110 0100` -> `70340F64`.

**Resultado:** La dirección física correspondiente es **`0x70340F64`**.
