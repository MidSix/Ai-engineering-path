# Boletín 4: Ejercicio 8

## Enunciado

*Un computador tiene una unidad de memoria de 8MB y una memoria caché de 4KB con un tamaño de línea de 256 bytes. Suponer que se hace una referencia a la dirección de memoria principal 0x006CA0.*

*a) Si la memoria caché utiliza correspondencia directa, ¿en qué línea de la memoria caché sería posible encontrar esa dirección? ¿Qué etiqueta habría que buscar?*
*b) Si la memoria caché utiliza correspondencia totalmente asociativa, ¿qué etiqueta habría que buscar?*
*c) Si la memoria caché utiliza asociatividad por conjuntos de 2 vías, ¿en qué conjunto debemos buscar? ¿qué etiqueta habría que buscar?*

---

## Datos Base y Dirección a Analizar

Primero, extraemos los datos comunes a todos los apartados.

*   **Tamaño de Memoria Principal (MP):** 8 MB
*   **Tamaño de la Caché:** 4 KB
*   **Tamaño de Línea (Bloque):** 256 bytes
*   **Dirección a analizar (Hex):** `0x006CA0`

### Cálculos Preliminares

#### Bits Totales de Dirección (MP)
*   **Cálculo:** `log₂(8 * 2^20) = log₂(2^23)`
*   **Resultado:** **23 bits**

#### Dirección en Binario
*   **Hex:** `006CA0`
*   **Binario (23 bits):**
    *   La dirección completa de 24 bits es `0000 0000 0110 1100 1010 0000`.
    *   Tomando los 23 bits menos significativos: `00 0000 0110 1100 1010 0000`.
    *   **Dirección a usar:** `0000000110110010100000`

#### Bits de Desplazamiento
Este campo es el mismo para todos los apartados, ya que depende solo del tamaño de línea.
*   **Cálculo:** `log₂(256)`
*   **Resultado:** **8 bits**

---

## a) Correspondencia Directa

En este modo, cada bloque de memoria principal solo puede ir a una línea específica de la caché.

#### a.1) Cálculo de Campos

Necesitamos el **índice de línea** para saber a qué línea va, y la **etiqueta** para verificar.

*   **Nº de Líneas en Caché:** `Tamaño Caché / Tamaño Línea = 4 KB / 256 B = 4096 / 256 = 16 líneas`.
*   **Bits de Índice:** `log₂(16)` -> **4 bits**.
*   **Bits de Etiqueta:** `Total - Índice - Desplazamiento = 23 - 4 - 8` -> **11 bits**.

El formato de la dirección es: **[Etiqueta (11 bits) | Índice (4 bits) | Desplazamiento (8 bits)]**

#### a.2) Análisis de la Dirección

Particionamos la dirección binaria `0000000110110010100000` con el formato `[11 | 4 | 8]`:

`00000001101` | `1001` | `01000000`
|:---:|:---:|:---:|
| **Etiqueta** | **Índice** | Desplazamiento |

*   **Línea de la caché:** El campo índice nos da la línea.
    *   Índice binario: `1001`
    *   Índice decimal: **Línea 9**.
*   **Etiqueta a buscar:** El campo etiqueta es el que se almacenaría y buscaría.
    *   Etiqueta binaria: `00000001101`
    *   Etiqueta hexadecimal: `0x00D`.

#### a.3) Respuesta

*   Sería posible encontrar la dirección en la **línea 9** de la caché.
*   Habría que buscar la etiqueta **`0x00D`**.
