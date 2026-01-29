# Estructura de la Entrada de Directorio de Caché: Etiqueta y Bits de Control

#tags: cache, arquitectura, memoria, control

El **Directorio de la Caché** es un componente fundamental para el funcionamiento de cualquier caché. Es una memoria muy pequeña y rápida que almacena metadatos sobre el contenido de cada línea de la caché. Cada entrada en este directorio corresponde a una línea de caché y contiene la información necesaria para determinar si una dirección solicitada es un acierto (hit) o un fallo (miss), y cómo proceder en cada caso.

Los elementos esenciales que componen una entrada típica en el directorio de la caché son:

---
## 1. Etiqueta (Tag)

*   **Definición:** Son los bits más significativos de la dirección de memoria principal que no son utilizados ni por el índice (si existe) ni por el desplazamiento.
*   **Propósito:** La Etiqueta identifica de forma única qué bloque específico de la memoria principal está actualmente almacenado en esa línea de caché. Cuando la CPU solicita un dato, el hardware de la caché compara la Etiqueta de la dirección solicitada con la Etiqueta almacenada en el directorio para la línea (o conjunto) correspondiente. Si coinciden, es un potencial acierto.

---
## 2. Bit de Validez (BV - Valid Bit)

*   **Definición:** Un bit simple (0 o 1).
*   **Propósito:** Indica si el contenido de la línea de caché es "válido" o no.
    *   `BV = 0`: La línea está vacía o contiene datos obsoletos/inválidos (por ejemplo, al iniciar el sistema o tras una invalidación).
    *   `BV = 1`: La línea contiene un bloque de datos válido de la memoria principal.
*   **Funcionamiento:** Cualquier comparación de Etiqueta solo tiene sentido si el Bit de Validez está a `1`. Si el BV es `0`, cualquier acceso a esa línea resultará en un fallo, incluso si la Etiqueta de la dirección coincide.

---
## 3. Bit de Modificación (BM - Dirty Bit)

*   **Definición:** Un bit simple (0 o 1). Este bit es **exclusivo** para las políticas de escritura de tipo **"Write-Back" (Post-escritura)**.
*   **Propósito:** Indica si el bloque de datos almacenado en la línea de caché ha sido modificado por la CPU desde que fue cargado de la memoria principal.
    *   `BM = 0`: El contenido de la línea de caché es idéntico al de la memoria principal (está "limpio").
    *   `BM = 1`: El contenido de la línea de caché ha sido modificado y es diferente al de la memoria principal (está "sucio").
*   **Funcionamiento:** Si un bloque sucio (`BM = 1`) es seleccionado para ser reemplazado por un nuevo bloque, sus datos deben ser escritos de vuelta a la memoria principal **antes** de que el nuevo bloque sea cargado en esa línea. Esto asegura la coherencia de los datos entre la caché y la memoria principal.

---
## Otros Bits de Control (Opcionales)

Dependiendo de la complejidad de la caché y el algoritmo de reemplazo, una entrada de directorio podría incluir otros bits, como:
*   **Bits de Uso (Usage/Reference Bits):** Utilizados por algoritmos como LRU (Least Recently Used) para determinar qué bloque ha sido el menos usado y es el mejor candidato para ser reemplazado.

La combinación de la Etiqueta y los Bits de Control (principalmente BV y BM) permite al hardware de la caché gestionar eficientemente la jerarquía de memoria, optimizando el rendimiento al minimizar los accesos lentos a la memoria principal.

---
**Ejercicio Relacionado:**
*   [[practica4_2_ejercicio3.md]] (Este ejercicio ilustra el uso del Bit de Modificación en una caché de post-escritura.)
