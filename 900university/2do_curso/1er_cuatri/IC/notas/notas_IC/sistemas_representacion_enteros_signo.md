# Sistemas de Representación de Enteros con Signo

Para representar números enteros tanto positivos como negativos en sistema binario, se han desarrollado varios esquemas. Los más comunes son Signo-Magnitud, Complemento a 1 (CA1) y Complemento a 2 (CA2).

## El Hilo Común: El Bit de Signo (MSB)

A pesar de sus diferencias operativas, los tres sistemas comparten una convención fundamental: el **Bit Más Significativo (MSB)**, el que está más a la izquierda, se utiliza como indicador del signo del número.

*   **MSB = 0**: El número es **positivo**.
*   **MSB = 1**: El número es **negativo**.

La diferencia entre los sistemas radica en cómo se interpretan los bits restantes una vez que se ha establecido el signo.

## 1. Signo-Magnitud

Es el sistema más intuitivo para los humanos.
*   **Representación**: El MSB es el signo y el resto de los bits representan la magnitud (el valor absoluto) del número, igual que en binario sin signo.
*   **Ejemplo (4 bits)**:
    *   `+5` es `0101` -> Signo `0` (+), Magnitud `101` (5).
    *   `-5` es `1101` -> Signo `1` (-), Magnitud `101` (5).
*   **Inconvenientes**:
    *   **Doble Cero**: Existen dos representaciones para el cero: `0000` (+0) y `1000` (-0).
    *   **Aritmética Compleja**: La suma y la resta requieren lógica adicional. El hardware debe comprobar los signos de los operandos para decidir si sumar o restar las magnitudes.

## 2. Complemento a 1 (CA1)

Fue un intento de simplificar la aritmética.
*   **Representación**: Para encontrar el opuesto de un número, se invierten **todos** sus bits.
*   **Ejemplo (4 bits)**:
    *   `+5` es `0101`.
    *   `-5` se obtiene invirtiendo todos los bits de `+5`: `1010`.
*   **Inconvenientes**:
    *   **Doble Cero**: Sigue existiendo el problema del doble cero. `0000` (+0) y su complemento `1111` (-0).
    *   **Aritmética**: La suma es más simple que en Signo-Magnitud, pero a veces requiere un paso adicional (sumar el acarreo final al resultado).

## 3. Complemento a 2 (CA2)

Es el estándar de facto en la computación moderna para representar enteros con signo.
*   **Representación**: Para encontrar el opuesto de un número, se invierten **todos** sus bits y **se suma 1** al resultado. (`CA2(x) = CA1(x) + 1`).
*   **Ejemplo (4 bits)**:
    *   `+5` es `0101`.
    *   Para `-5`: Invertir (`1010`) y sumar 1, resultando en `1011`.
*   **Ventajas**:
    *   **Cero Único**: Solo hay una representación para el cero (`0000`).
    *   **Aritmética Simplificada**: La suma y la resta se realizan con el mismo circuito sumador, tratando los números de forma uniforme sin importar su signo. `A - B` se calcula como `A + (-B)`. Esto simplifica enormemente el diseño del hardware (ALU).

## Tabla Comparativa Resumen

| Característica | Signo-Magnitud | Complemento a 1 (CA1) | Complemento a 2 (CA2) |
| :--- | :--- | :--- | :--- |
| **Relación de Cálculo**| Independiente | `CA2 = CA1 + 1` | Derivado directo de CA1 |
| **Uso del MSB como Signo** | **Sí** | **Sí** | **Sí** |
| **Cálculo del Negativo** | Cambiar solo el MSB | Invertir **todos** los bits | Invertir todos los bits y **sumar 1** |
| **Representación del Cero**| Doble (`+0` y `-0`)| Doble (`+0` y `-0`)| **Única** |
| **Uso en Cómputo**| Poco común (mantisa en IEEE 754) | Obsoleto | **Estándar actual** |

---

## Notas Relacionadas

*   [[representacion_direcciones_vs_valores]]
