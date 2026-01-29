# Aritmética Binaria: Atajos para Sumar y Restar 1

Este apunte describe los "trucos" o atajos para sumar y restar 1 a un número binario, explicando la lógica subyacente basada en el acarreo y el préstamo.

## Sumar 1

**El atajo:** Empezando desde la derecha, cambia todos los `1` por `0` hasta que encuentres el primer `0`. Cambia ese `0` por un `1` y no toques nada más a la izquierda.

**La Lógica: El Acarreo (Carry)**

La suma `1 + 1` en binario es `0` y te llevas un `1` (acarreo) a la siguiente columna. Este acarreo se propaga hacia la izquierda.

**Ejemplo:** `11000110001101 + 1`

```
  11000110001101
+              1
------------------
```

1.  **Bit 0 (LSB):** `1 + 1 = 0`, y me llevo 1.
2.  **Bit 1:** Era un `0`, pero le sumo el acarreo. `0 + 1 = 1`. El acarreo se detiene aquí.
3.  **Resto:** Los demás bits no se ven afectados.

**Resultado:** `11000110001110`

El atajo funciona porque la cadena de acarreos convierte en `0` a todos los `1` que encuentra, hasta que un `0` absorbe el acarreo final convirtiéndose en `1`.

---

## Restar 1

**El atajo:** Empezando desde la derecha, busca el primer `1`. Cámbialo por un `0` y cambia todos los `0` que te hayas saltado a su derecha por `1`.

**La Lógica: El Préstamo (Borrow)**

La resta `0 - 1` en binario es `1` y pides prestado un `1` a la siguiente columna de la izquierda. Este préstamo puede propagarse.

**Ejemplo 1 (simple):** `11000110001101 - 1`

El bit menos significativo es `1`, así que `1 - 1 = 0`. No hay préstamo.

**Resultado:** `11000110001100`

**Ejemplo 2 (con préstamo):** `110100 - 1`

```
  110100
-      1
----------
```
1.  **Bit 0 (LSB):** `0 - 1`. No se puede. Pide prestado.
2.  **Bit 1:** Es `0`, no puede prestar. A su vez, pide prestado.
3.  **Bit 2:** Es `1`. ¡Puede prestar!
    *   Este `1` se convierte en `0`.
    *   El bit 1 (que era `0`) se convierte en `10`. Presta `1` a su derecha y se queda como `1`.
    *   El bit 0 (que era `0`) recibe el préstamo y se convierte en `10`.
4.  **Realizamos la resta:**
    *   **Bit 0:** `10 - 1 = 1`.
    *   **Bit 1:** Se quedó como `1`. `1 - 0 = 1`.
    *   **Bit 2:** Se quedó como `0`. `0 - 0 = 0`.
    *   El resto no cambia.

**Resultado:** `110011`

El atajo funciona porque el primer `1` que encuentras (desde la derecha) es el que "paga" la deuda, convirtiéndose en `0`. El préstamo se propaga por los `0`s intermedios, convirtiéndolos en `1`s en el proceso.
