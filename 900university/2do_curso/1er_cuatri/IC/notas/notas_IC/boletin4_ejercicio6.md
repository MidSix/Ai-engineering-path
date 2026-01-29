# Resolución del Ejercicio 6 - Boletín 4: Memoria

Este documento detalla la resolución del ejercicio 6 del boletín del tema 4, explicando los conceptos teóricos necesarios y los cálculos aplicados.

## Enunciado del Ejercicio

> Un computador dispone de un sistema de memoria central constituido por una memoria principal Mp y una cache Mc. Mp tiene una dimensión de 128K palabras y está estructurada como un conjunto de módulos de 8K palabras con entrelazado de orden inferior. La Mc tiene un tamaño de 2K palabras con líneas de 256 palabras y una correspondencia directa. Se pide:
>
> a) Número de módulos de Mp.
> b) Interpretación de los bits de la dirección física del sistema de memoria para Mp.
> c) Interpretación de los bits de la dirección física del sistema de memoria para Mc.

---

## 1. Conceptos Teóricos Clave

### Memoria Principal (Mp): Módulos y Bloques

*   **Dirección Física**: El tamaño de la Mp (128K palabras) define el tamaño de la dirección. `128K = 2^17`, por lo que las direcciones físicas son de **17 bits**.

*   **División en Módulos**: Es una división **física/organizativa** de la RAM para mejorar el rendimiento (paralelismo). En este caso, la Mp se organiza en 16 módulos de 8K palabras.

*   **División en Bloques**: Es una división **lógica** que la caché impone sobre el espacio de direcciones de la Mp. La RAM no "sabe" de bloques.
    *   Un **bloque** es la unidad de transferencia entre RAM y Caché. Su tamaño es igual al de la línea de caché (256 palabras).
    *   Esta división es una **cuadrícula fija y estática**. El Bloque 0 contiene las direcciones 0-255, el Bloque 1 las 256-511, y así sucesivamente. Una palabra siempre pertenece a un único bloque.

### Funcionamiento Detallado: El Proceso de Acceso a Caché

Es fundamental entender que en un sistema informático moderno, existe **un único espacio de direcciones físicas**. La CPU, la caché y la memoria principal (RAM) operan todas dentro de este mismo mapa de direcciones. Esto significa que si la memoria principal usa direcciones de 17 bits (como en este ejercicio), la CPU generará direcciones de 17 bits, y la caché analizará y manipulará esas mismas direcciones de 17 bits. No hay sistemas de direccionamiento separados.

#### El Origen de la Etiqueta, Índice y Desplazamiento

El procesador no sabe de etiquetas o índices. Cuando necesita un dato, genera una **única dirección física completa** (de 17 bits en este caso). Es el **controlador de la caché** el que recibe esta dirección y la **divide lógicamente** en tres partes según su configuración:

```
Dirección Física Completa (17 bits)
|---------- Etiqueta (6) ----------|-- Índice (3) --|-- Desplazamiento (8) --|
```
La etiqueta, el índice y el desplazamiento no se "buscan", se **extraen** de la dirección original.

#### El Problema: Competición por las Líneas de Caché

La caché es mucho más pequeña que la RAM. En este ejercicio:
*   **RAM**: Tiene `128K / 256 = 512` bloques de memoria en total.
*   **Caché**: Tiene solo `2K / 256 = 8` líneas disponibles.

Esto significa que `512 / 8 = 64` bloques diferentes de la RAM compiten por usar la misma línea de caché. La **Etiqueta** es el mecanismo indispensable para saber cuál de los 64 posibles bloques es el que está **actualmente** en la línea.

#### El Proceso Completo (Hit/Miss)

El acceso a la caché sigue un orden estricto:

1.  **Selección por Índice**: El controlador usa los **bits del Índice** de la dirección para ir a la única línea posible (p. ej., Índice `101` -> va a la línea 5).

2.  **Comprobación de Validez**: Una vez en la línea, mira el **Bit de Válido**.
    *   **Si es 0 (Inválido)**: Es un **FALLO (Compulsory Miss)**. La línea está vacía o su contenido no es fiable (ej: al encender el PC). El proceso salta al paso 5.
    *   **Si es 1 (Válido)**: El contenido es potencialmente correcto. Se procede al siguiente paso.

3.  **Verificación de Etiqueta**: El controlador compara la **Etiqueta** extraída de la dirección con la `Etiqueta almacenada` en la línea.
    *   **Si no coinciden**: Es un **FALLO (Conflict Miss)**. La línea está ocupada por otro de los 64 bloques que compiten por ella. El proceso salta al paso 5.
    *   **Si coinciden**: ¡**ACIERTO (Hit)**! El bloque correcto está en la caché.

4.  **Selección por Desplazamiento**: En caso de acierto, se usan los **bits del Desplazamiento** para seleccionar la palabra exacta de entre las 256 que hay en el bloque de datos de la línea. El dato se envía a la CPU.

5.  **Gestión de Fallo**: Si ha habido un fallo (pasos 2 o 3), el controlador pide a la RAM el bloque completo. La dirección de inicio del bloque se forma con `[Etiqueta | Índice | 00...0]`. La RAM envía el bloque, que se almacena en la línea, se actualiza la `Etiqueta almacenada` y se pone el `Bit de Válido` a 1. Finalmente, se entrega el dato a la CPU.

---

## 2. Resolución del Ejercicio

*(La resolución del ejercicio no cambia, ya que se basa en la aplicación de estos conceptos teóricos)*

### a) Número de módulos de Mp
*   **Cálculo**: `128K / 8K = 16`
*   **Respuesta**: **16 módulos**.

### b) Interpretación de los bits de dirección para Mp
Con 17 bits de dirección y 16 módulos (`2^4`), y entrelazado de orden inferior:
*   **4 bits menos significativos**: Selección de Módulo.
*   **13 bits más significativos**: Dirección en el Módulo.
```
      <---- 13 bits ----> <--- 4 bits --->
|-----------------------|---------------|
| Dirección en el Módulo| Selecc. Módulo|
|-----------------------|---------------|
```

### c) Interpretación de los bits de dirección para Mc
Para una caché de 2K palabras con líneas de 256 palabras (8 líneas en total):
*   **Desplazamiento**: `log2(256) = 8 bits`.
*   **Índice**: `log2(8) = 3 bits`.
*   **Etiqueta**: `17 - 8 - 3 = 6 bits`.
```
<-- 6 bits --> <-- 3 bits --> <---- 8 bits ---->
|--------------|--------------|------------------|
|   Etiqueta   |    Índice    |  Desplazamiento  |
|--------------|--------------|------------------|
```

---
## Vínculos
* La resolución de este ejercicio se basa en los principios de la [[correspondencia_directa]].