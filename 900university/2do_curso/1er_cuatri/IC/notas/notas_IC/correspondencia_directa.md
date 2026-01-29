# Correspondencia Directa en Memoria Caché

La correspondencia directa es un mecanismo de ubicación de bloques en la memoria caché. Su característica principal es que cada bloque de la memoria principal solo puede alojarse en **una única línea específica** de la caché. No tiene otra opción.

La línea de caché destino se determina mediante la fórmula:

>Cuando hablamos de bloque de memoria no estamos hablando de **modulos**, los bloques de memoria son la forma en que la cache ve a la memoria principal, lo forma en que esta extrae informacion de ella.

`Nº de línea de caché = (Nº de bloque de memoria) % (Nº total de líneas en la caché)`

## Flujo de Acceso

Cuando la CPU solicita una dirección de memoria, ocurren los siguientes pasos:

1.  **Descomposición de la Dirección**: La dirección se divide en tres campos: **Etiqueta**, **Índice** y **Desplazamiento**.
    *   **Índice**: Indica la única línea de la caché donde puede estar el bloque.
    *   **Etiqueta**: Identifica qué bloque de memoria (de todos los que pueden ir a esa línea) es el que está cargado actualmente.
    *   **Desplazamiento**: Señala la posición de la palabra o byte deseado dentro del bloque.

2.  **Búsqueda en Caché**: Se usa el **Índice** para acceder directamente a la línea candidata.

3.  **Verificación (Hit/Miss)**: Una vez en la línea, el hardware de la caché comprueba **en paralelo** dos condiciones:
    *   Que el **bit de validez** de la línea sea `1` (indicando que la información no es basura).
    *   Que la **Etiqueta** de la dirección solicitada por la CPU coincida con la etiqueta almacenada en esa línea de caché.

4.  **Resultado**:
    *   **Acierto (Hit)**: Si ambas condiciones son ciertas, el dato es correcto. Se utiliza el **Desplazamiento** para encontrar la palabra exacta dentro del bloque de datos y se envía a la CPU.
    *   **Fallo (Miss)**: Si alguna de las condiciones es falsa, el dato no está en la caché. Se debe traer el bloque completo desde la memoria principal, cargarlo en la línea indicada por el índice, actualizar la etiqueta, poner el bit de validez a 1 y, finalmente, entregar el dato a la CPU.

### Una Nota Importante: La Granularidad del Bit de Validez

Es crucial entender que **hay un único bit de validez por cada línea de caché**. No existen bits de validez individuales para cada palabra dentro de la línea.

La razón es que la unidad mínima de transferencia entre la memoria principal y la caché es el **bloque**. Cuando se produce un fallo y se trae información de la RAM, se copia el bloque entero en la línea de la caché. Por lo tanto, la validez se gestiona a nivel de bloque/línea:

*   Si el bit de validez es `1`, se considera que **todo el bloque** contenido en la línea es una copia coherente de lo que había en memoria principal en el momento de la carga.
*   Si es `0`, toda la línea se considera inválida.

Esta decisión de diseño simplifica enormemente el hardware. La comprobación del bit de validez se realiza **antes** de usar el desplazamiento, ya que valida la línea completa, no una parte de ella.

## Vínculos

*   Este concepto es fundamental para resolver el [[boletin4_ejercicio6]].
