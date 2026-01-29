# Boletín 4: Ejercicio 7

## Enunciado

*Una memoria caché asociativa por conjuntos consta de 16 conjuntos con 4 vías por conjunto, con un tamaño de línea de 512 bytes. La memoria principal tiene una capacidad de 4MB. A la dirección de memoria principal, 0x2864C0, ¿qué conjunto le corresponde (expresado en decimal)?*

---

## 1. Extracción de Datos

*   **Tipo de correspondencia:** Asociativa por conjuntos.
*   **Nº de Conjuntos:** 16
*   **Vías por conjunto (Líneas por conjunto):** 4
*   **Tamaño de Línea (Tamaño de Bloque):** 512 bytes
*   **Tamaño de Memoria Principal (MP):** 4 MB
*   **Dirección a analizar:** `0x2864C0`

---

## 2. Cálculo de los Campos de la Dirección

Para determinar a qué conjunto corresponde la dirección, primero necesitamos particionar la dirección de memoria en los campos **Etiqueta**, **Índice** y **Desplazamiento**.

> Para un repaso de cómo se calculan estos campos, ver [[calculo_campos_direccion_cache|Cómo Calcular los Campos de la Dirección de Caché]].

#### Paso 2.1: Bits Totales de la Dirección

El tamaño de la memoria principal determina el número total de bits de una dirección.
*   **Cálculo:** `log₂(Tamaño MP) = log₂(4 MB) = log₂(4 * 2^20 bytes) = log₂(2^2 * 2^20) = log₂(2^22)`
*   **Resultado:** **22 bits**

#### Paso 2.2: Bits de Desplazamiento

Determinado por el tamaño del bloque/línea, para poder direccionar cada byte dentro de él.
*   **Cálculo:** `log₂(Tamaño de Bloque) = log₂(512 bytes)`
*   **Resultado:** **9 bits**

#### Paso 2.3: Bits de Índice de Conjunto

Determinado por el número de conjuntos, para poder seleccionar uno de ellos.
*   **Cálculo:** `log₂(Nº de Conjuntos) = log₂(16)`
*   **Resultado:** **4 bits**

#### Paso 2.4: Bits de Etiqueta

Son los bits restantes, que sirven para identificar el bloque de memoria dentro del conjunto.
*   **Cálculo:** `Bits Totales - Bits de Índice - Bits de Desplazamiento = 22 - 4 - 9`
*   **Resultado:** **9 bits**

---

## 3. Formato Final de la Dirección

La dirección de 22 bits se descompone de la siguiente manera:

| Etiqueta (9 bits) | Índice (4 bits) | Desplazamiento (9 bits) |
| :--- | :--- | :--- |

---

## 4. Resolución del Ejemplo

Ahora aplicamos este formato a la dirección dada.

#### Paso 4.1: Convertir la dirección a binario

*   **Dirección Hexadecimal:** `0x2864C0`
*   **Dirección Binaria (22 bits):**
    *   `2` -> `0010`
    *   `8` -> `1000`
    *   `6` -> `0110`
    *   `4` -> `0100`
    *   `C` -> `1100`
    *   `0` -> `0000`
    *   Juntando: `0010 1000 0110 0100 1100 0000` (24 bits).
    *   Como la dirección es de 22 bits, tomamos los 22 bits menos significativos: `10 1000 0110 0100 1100 0000`.
    *   Esta es la dirección completa: `1010000110010011000000`

#### Paso 4.2: Particionar la dirección binaria

Aplicamos nuestro formato `[9 | 4 | 9]` a la dirección binaria:

`101000011` | `0010` | `011000000`
|:---:|:---:|:---:|
| Etiqueta | **Índice** | Desplazamiento |

#### Paso 4.3: Identificar el conjunto

El campo del índice nos dice a qué conjunto corresponde la dirección.
*   **Índice en binario:** `0010`
*   **Índice en decimal:** `2`

---

## 5. Respuesta Final

La dirección `0x2864C0` corresponde al **conjunto 2**.

---
## Vínculos

*   [[tipos_de_correspondencia_cache]]
*   [[calculo_campos_direccion_cache]]
