# Solución Ejercicio 6 - Práctica 4.1: Memoria Entrelazada

El problema nos presenta una memoria principal con la siguiente configuración:
*   **Número de módulos:** 4
*   **Tamaño de cada módulo:** 8 palabras
*   **Capacidad total:** 4 módulos * 8 palabras/módulo = 32 palabras.
*   **Rango de direcciones:** De 0 a 31.
*   **Secuencia de direcciones a leer:** 07, 08, 09, 16, 05, 10, 22, 23, 24, 25.

El objetivo es calcular el rendimiento (palabras por ciclo) que se obtiene para esta secuencia con dos tipos de entrelazamiento de memoria. El principio clave es que se pueden realizar accesos en paralelo a **diferentes** módulos de memoria, pero los accesos al **mismo** módulo deben ser secuenciales. El número de ciclos necesarios vendrá determinado por el módulo que reciba más solicitudes de acceso.

---

### a) Entrelazamiento de Orden Superior (High-Order Interleaving)

En este modo, los bits más significativos de la dirección se usan para seleccionar el módulo, y los menos significativos para seleccionar la palabra dentro de ese módulo.

*   **Bits para el módulo:** `log2(4) = 2` bits.
*   **Bits para el desplazamiento (offset):** `log2(8) = 3` bits.
*   **Formato de dirección (5 bits):** `MM DDD` (M = bit de Módulo, D = bit de Desplazamiento).
*   **Cálculo del módulo:** `Módulo = floor(Dirección / 8)`

Calculamos a qué módulo corresponde cada dirección:
*   `07`: floor(7/8) = **Módulo 0**
*   `08`: floor(8/8) = **Módulo 1**
*   `09`: floor(9/8) = **Módulo 1**
*   `16`: floor(16/8) = **Módulo 2**
*   `05`: floor(5/8) = **Módulo 0**
*   `10`: floor(10/8) = **Módulo 1**
*   `22`: floor(22/8) = **Módulo 2**
*   `23`: floor(23/8) = **Módulo 2**
*   `24`: floor(24/8) = **Módulo 3**
*   `25`: floor(25/8) = **Módulo 3**

Agrupamos las peticiones por módulo:
*   **Módulo 0:** 05, 07 (2 accesos)
*   **Módulo 1:** 08, 09, 10 (3 accesos)
*   **Módulo 2:** 16, 22, 23 (3 accesos)
*   **Módulo 3:** 24, 25 (2 accesos)

El número de ciclos necesarios es el máximo de accesos a un único módulo, que en este caso es **3** (para los módulos 1 y 2).

*   **Ciclo 1:** Se lee una palabra de cada módulo: 05 (M0), 08 (M1), 16 (M2), 24 (M3). (4 palabras)
*   **Ciclo 2:** Se leen las siguientes palabras de cada módulo: 07 (M0), 09 (M1), 22 (M2), 25 (M3). (4 palabras)
*   **Ciclo 3:** Se leen las palabras restantes: 10 (M1), 23 (M2). (2 palabras)

Se leen un total de 10 palabras en 3 ciclos.
**Rendimiento:** 10 palabras / 3 ciclos = **3.33 palabras/ciclo**.

---

### b) Entrelazamiento de Orden Inferior (Low-Order Interleaving)

En este modo, los bits menos significativos de la dirección seleccionan el módulo. Esto provoca que direcciones consecutivas se almacenen en módulos distintos, ideal para acceso a vectores.

*   **Cálculo del módulo:** `Módulo = Dirección % 4` (El resto de la división por el número de módulos).

Calculamos a qué módulo corresponde cada dirección:
*   `07`: 7 % 4 = **Módulo 3**
*   `08`: 8 % 4 = **Módulo 0**
*   `09`: 9 % 4 = **Módulo 1**
*   `16`: 16 % 4 = **Módulo 0**
*   `05`: 5 % 4 = **Módulo 1**
*   `10`: 10 % 4 = **Módulo 2**
*   `22`: 22 % 4 = **Módulo 2**
*   `23`: 23 % 4 = **Módulo 3**
*   `24`: 24 % 4 = **Módulo 0**
*   `25`: 25 % 4 = **Módulo 1**

Agrupamos las peticiones por módulo:
*   **Módulo 0:** 08, 16, 24 (3 accesos)
*   **Módulo 1:** 05, 09, 25 (3 accesos)
*   **Módulo 2:** 10, 22 (2 accesos)
*   **Módulo 3:** 07, 23 (2 accesos)

De nuevo, el número máximo de accesos a un solo módulo es **3**, por lo que se requieren **3 ciclos**.

*   **Ciclo 1:** 08 (M0), 05 (M1), 10 (M2), 07 (M3). (4 palabras)
*   **Ciclo 2:** 16 (M0), 09 (M1), 22 (M2), 23 (M3). (4 palabras)
*   **Ciclo 3:** 24 (M0), 25 (M1). (2 palabras)

Se leen 10 palabras en 3 ciclos.
**Rendimiento:** 10 palabras / 3 ciclos = **3.33 palabras/ciclo**.
