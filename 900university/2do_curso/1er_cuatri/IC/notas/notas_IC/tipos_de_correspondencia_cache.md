# Tipos de Correspondencia en la Memoria Caché

La correspondencia (o mapping) define las reglas sobre dónde puede colocarse un bloque de memoria principal dentro de la caché. Existen tres estrategias principales, cada una con un compromiso diferente entre coste de hardware y probabilidad de aciertos.

> **Nota Fundamental:** Para entender cómo se divide la dirección de memoria en los campos `Etiqueta`, `Índice` y `Desplazamiento` para cada tipo de correspondencia, consulta el siguiente apunte:
> [[calculo_campos_direccion_cache|Cómo Calcular los Campos de la Dirección de Caché]]

---

### 1. Correspondencia Directa

Es el método más simple y restrictivo.

*   **Regla Principal:** Cada bloque de memoria principal solo puede ir a **una única línea específica** de la caché.
*   **División de la Dirección:** Se usan tres campos:
    *   **Etiqueta**: Identificador del bloque.
    *   **Índice de Línea**: Campo clave que determina la única línea de la caché donde puede residir el bloque. `Nº de línea = (Nº de bloque) % (Nº total de líneas)`.
    *   **Desplazamiento**: Ubica la palabra dentro del bloque.
*   **Búsqueda:** Se usa el **Índice de Línea** para saltar directamente a la línea candidata y se comprueba la etiqueta.
*   **Ventaja:** Rápido y barato (un solo comparador).
*   **Desventaja:** Propenso a **fallos de conflicto**: si dos bloques de uso frecuente mapean a la misma línea, se expulsarán mutuamente de forma constante, incluso si el resto de la caché está vacía.

> [[correspondencia_directa|Ver apunte detallado de Correspondencia Directa]]

---

### 2. Correspondencia Totalmente Asociativa

El método más flexible y costoso.

*   **Regla Principal:** Un bloque de memoria principal puede ir a **cualquier línea** de la caché.
*   **División de la Dirección:** Solo dos campos:
    *   **Etiqueta**: Identificador del bloque (ocupa la mayor parte de la dirección).
    *   **Desplazamiento**: Ubica la palabra dentro del bloque.
*   **Búsqueda:** El hardware debe comparar la etiqueta de la dirección buscada con las etiquetas de **TODAS** las líneas de la caché **en paralelo**.
*   **Ventaja:** Elimina por completo los fallos de conflicto. Tiene la tasa de aciertos teórica más alta.
*   **Desventaja:** Extremadamente caro y lento. Requiere un comparador de hardware por cada línea de caché, lo que lo hace inviable para cachés de gran tamaño.

---

### 3. Correspondencia Asociativa por Conjuntos

La solución de compromiso y la más utilizada en la práctica.

*   **Regla Principal:** La caché se divide en **conjuntos** (grupos de `N` líneas). Un bloque de memoria puede ir a **cualquier línea, pero solo dentro de un conjunto específico**.
*   **División de la Dirección:** Vuelve a tener tres campos, pero con una diferencia crucial respecto a la directa:
    *   **Etiqueta**: Identificador del bloque.
    *   **Índice de Conjunto**: Determina el **único conjunto** donde puede residir el bloque. `Nº de conjunto = (Nº de bloque) % (Nº total de conjuntos)`.
    *   **Desplazamiento**: Ubica la palabra dentro del bloque.
*   **Búsqueda:** Es un proceso híbrido:
    1.  Se usa el **Índice de Conjunto** para saltar directamente al conjunto candidato.
    2.  Se compara en paralelo la etiqueta de la dirección con las etiquetas de las **`N` líneas** que forman ese conjunto.
*   **Ventaja:** Excelente equilibrio entre tasa de aciertos y coste de hardware. Reduce significativamente los fallos de conflicto frente a la correspondencia directa sin el coste desorbitado de la totalmente asociativa.
*   **Desventaja:** Más compleja que la directa. Aún puede haber fallos de conflicto si más de `N` bloques de uso frecuente mapean al mismo conjunto.

### Tabla Comparativa

| Característica | **Correspondencia Directa** | **Asociativa por Conjuntos (N-vías)** | **Totalmente Asociativa** |
| :--- | :--- | :--- | :--- |
| **Destino del Bloque**| 1 línea específica. | N líneas dentro de 1 conjunto específico. | Cualquier línea de toda la caché. |
| **Campo de Índice**| **Índice de Línea**: Apunta a una LÍNEA. | **Índice de Conjunto**: Apunta a un CONJUNTO. | No existe. |
| **Comparaciones Paralelas**| 1 | N | Todas las líneas de la caché |
| **Fallos de Conflicto** | Muy probables. | Menos probables. | Imposibles. |
| **Coste/Complejidad** | Bajo. | Medio. | Muy alto. |

---
## Vínculos

* Este apunte se relaciona con [[correspondencia_directa]] y es necesario para entender el contexto de ejercicios como [[boletin4_ejercicio6]].

---

### Nota Aclaratoria: Interpretación de los Diagramas del Tema 4

Es crucial entender que los diagramas de memoria caché presentados en el **Tema 4 (`T4 - 1Memoria.pdf`)** son **simplificaciones pedagógicas** diseñadas para facilitar la comprensión de los mecanismos de correspondencia.

**La Simplificación Principal:**

*   **Cada "fila" en los diagramas de la caché representa una única y completa `línea de caché`**.

**La Realidad Detrás de la Simplificación:**

*   Una `línea de caché` no es solo una "fila", sino la **unidad indivisible de almacenamiento** que contiene un **`bloque` de datos completo** (por ejemplo, 32, 64 o 128 bytes) traído desde la memoria principal.
*   El diagrama simplifica esto para no tener que dibujar todos los bytes en cada línea. En su lugar, la "fila" representa la entrada completa: los metadatos (bit de validez, etiqueta) y el bloque de datos asociado a ella.

**Conclusión Clave:**

Tu razonamiento es **correcto**. Cuando veas una "fila" en esos diagramas, debes interpretarla como una `línea de caché` que contiene un `bloque de memoria`. Por tanto:
*   En **correspondencia asociativa por conjuntos**, un conjunto es efectivamente una agrupación de estas líneas (que a su vez contienen bloques).
*   En **correspondencia directa**, el "índice de línea" de la dirección apunta a una de estas líneas/bloques.
