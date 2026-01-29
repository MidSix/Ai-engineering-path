# Cálculo de los Campos de la Dirección de Caché

La tarea fundamental del hardware de la caché es tomar una dirección de memoria física completa y descomponerla en campos (trozos) para determinar si el dato reside en la caché y dónde. Estos campos son la **Etiqueta**, el **Índice** y el **Desplazamiento**.

Existen dos perspectivas equivalentes para entender este cálculo:

1.  **Top-Down (por sustracción):** Se calculan los campos más "fáciles" (Índice y Desplazamiento) y la Etiqueta es simplemente lo que sobra de la dirección total.
2.  **Bottom-Up (por dirección de bloque):** Se aísla primero la "Dirección de Bloque" y luego se particiona en Etiqueta e Índice. Esta perspectiva es más fundamental.

Esta nota se centrará en la segunda perspectiva.

---

### Paso 1 (Común a todos): Calcular el Desplazamiento

Este campo responde a la pregunta: *"¿Qué byte/palabra busco DENTRO de una linea de cache?"*

*   **Propósito:** Localizar un byte específico dentro de un bloque de datos.
*   **Cálculo:** Su tamaño depende exclusivamente del tamaño del bloque(linea de cache).
*   **Fórmula:** `Bits de Desplazamiento = log₂(Tamaño de la linea de cache en bytes)`

Este cálculo es **idéntico para los tres tipos de correspondencia**.

*Ejemplo: Para un bloque de 512 bytes, el desplazamiento es de `log₂(512) = 9 bits`.*

---

### Paso 2 (El Concepto Clave): La Dirección de Bloque

Una vez nos olvidamos del desplazamiento, los bits restantes de la dirección representan el número o identificador único del bloque en la memoria principal.

*   **Propósito:** Identificar de forma única un bloque en toda la Memoria Principal.
*  Cálculo:`Bits de Dirección de Bloque = log₂(Nº Total de Bloques en Memoria Principal)` 
	*  Donde `Nº Total de Bloques = Tamaño Memoria Principal / Tamaño de bloque`.
		* El tamaño de bloque es el tamaño de la linea de caché, la MP se divide en bloques de memoria desde el punto de vista de la cache y en el caso de que la RAM ya esté dividida en módulos esto no importa porque recuerda que necesariamente los módulos llevan un entrelazamiento ya sea superior o inferior, es decir, la RAM aunque en distintos módulos, está unida a través de un entrelazamiento por lo que desde el punto de vista de la caché la RAM puede o no estar modulada que para la caché la RAM seguirá siendo un bloque de memoria único que será dividido en bloques de memoria para que la caché pueda acceder a ella aprovechando los principios de localidad espacial y temporal. O sea, la caché puede en un bloque de memoria al que accede estar accediendo a recursos RAM que están en dos módulos diferentes, no pasa nada por esto debido al entrelazamiento.

**Esta "Dirección de Bloque" es la que se particiona en Etiqueta e Índice.**

`[---------- Dirección de Bloque ----------]`
`[----- Etiqueta ----- | ----- Índice -----]`

---

### Paso 3: Calcular Índice y Etiqueta (Varía por tipo de correspondencia)

Aquí repartimos los bits de la "Dirección de Bloque" según las reglas de cada tipo de caché.

#### a) Correspondencia Directa

*   **Índice:** Responde a: *"¿A qué LÍNEA específica de la caché va este bloque?"*
    *   **Fórmula:** `Bits de Índice = log₂(Nº total de LÍNEAS en la caché)`
*   **Etiqueta:** Son los bits restantes de la Dirección de Bloque. Sirven para verificar que el bloque que está en esa línea es el correcto.
    *   **Fórmula:** `Bits de Etiqueta = Bits de Dirección de Bloque - Bits de Índice`

#### b) Correspondencia Asociativa por Conjuntos

*   **Índice:** Responde a: *"¿A qué CONJUNTO específico de la caché va este bloque?"*
    *   **Fórmula:** `Bits de Índice = log₂(Nº total de CONJUNTOS en la caché)`
*   **Etiqueta:** Son los bits restantes de la Dirección de Bloque. Sirven para verificar cuál de las `N` líneas del conjunto contiene el bloque correcto.
    *   **Fórmula:** `Bits de Etiqueta = Bits de Dirección de Bloque - Bits de Índice`

#### c) Correspondencia Totalmente Asociativa

*   **Índice:** No existe. Un bloque puede ir a cualquier línea de la caché, por lo que no hay bits dedicados a "seleccionar" una posición.
    *   **Fórmula:** `Bits de Índice = 0`
*   **Etiqueta:** Al no haber índice, la Dirección de Bloque completa se convierte en la Etiqueta. El hardware debe comparar esta etiqueta con la de todas las líneas de la caché.
    *   **Fórmula:** `Bits de Etiqueta = Bits de Dirección de Bloque`

---
### Tabla Resumen

| Campo | Correspondencia Directa | Asociativa por Conjuntos | Totalmente Asociativa |
| :--- | :--- | :--- | :--- |
| **Desplazamiento** | `log₂(Tamaño Bloque)` | `log₂(Tamaño Bloque)` | `log₂(Tamaño Bloque)` |
| **Índice** | `log₂(Nº Líneas)` | `log₂(Nº Conjuntos)` | 0 (No existe) |
| **Etiqueta** | `Dirección Bloque - Índice` | `Dirección Bloque - Índice` | `Dirección Bloque` (Completa) |
| **Etiqueta (Fórmula Clásica)** | `Total - Índice - Desplazamiento` | `Total - Índice - Desplazamiento`| `Total - Desplazamiento` |

---
## Vínculos

*   Este apunte es fundamental para entender [[tipos_de_correspondencia_cache]], [[correspondencia_directa]] y para resolver ejercicios como [[boletin4_ejercicio6]].
