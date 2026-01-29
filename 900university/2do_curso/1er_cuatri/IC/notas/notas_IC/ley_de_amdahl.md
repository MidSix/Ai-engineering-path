# Ley de Amdahl

La **Ley de Amdahl** es un modelo que se utiliza para calcular la máxima mejora teórica del rendimiento global de un sistema cuando solo se mejora una parte de dicho sistema. Es fundamental para evaluar la viabilidad y el impacto de optimizaciones específicas.

## La Fórmula de la Ley de Amdahl

La aceleración (Speedup) total del sistema se calcula como:

$$ A = \frac{1}{(1 - F_m) + \frac{F_m}{A_m}} $$

Donde:

*   **`A` (Aceleración Total):** Es el factor por el cual el programa se ejecuta más rápido en el sistema mejorado comparado con el sistema original. Un valor de `A = 1.5` significa que el programa es 1.5 veces más rápido.
*   **`Fm` (Fracción Mejorada):** Es la **fracción del TIEMPO de ejecución total original** que la parte mejorada del sistema utilizaba.
    *   **¡Importante!** `Fm` no es la fracción de instrucciones mejoradas, sino la proporción del tiempo total de ejecución que esa parte representaba antes de la mejora. `0 ≤ Fm ≤ 1`.
*   **`Am` (Aceleración de la Mejora):** Es el factor por el cual la parte específica que se ha mejorado se ejecuta más rápido.
    *   Se calcula como: `Tiempo_original_de_la_parte_mejorada / Tiempo_nuevo_de_la_parte_mejorada`. `Am ≥ 1`.

## Implicación de la Ley de Amdahl

La ley de Amdahl nos muestra que la aceleración total de un sistema está limitada por la porción no mejorada del mismo. Si `Fm` es muy pequeño (pocas partes del programa se benefician de la mejora), incluso una `Am` muy grande (la mejora es espectacular) resultará en una `A` total modesta.

## Ejemplo Ilustrativo (Ejercicio 11a, Boletín 1)

**Enunciado:**
Se plantean dos modificaciones en el diseño de un procesador:
1.  Mejorar la ALU de enteros para que el CPI de las instrucciones aritmético-lógicas con enteros pase de 1 a 0.8.
2.  Mejorar el coprocesador en punto flotante para que las instrucciones en punto flotante se ejecuten al doble de velocidad.

En este procesador se suele ejecutar un **45% del tiempo** instrucciones aritmético-lógicas con enteros y el **10% del tiempo** de instrucciones en punto flotante.

**Objetivo:** Calcular la aceleración obtenida con cada una de estas mejoras.

---

### Mejora 1: ALU de Enteros

*   **Descripción:** El CPI de las instrucciones aritmético-lógicas con enteros pasa de 1 a 0.8.
*   **`Am` (Aceleración de la Mejora):**
    `Am = CPI_original / CPI_nuevo = 1 / 0.8 = 1.25`
*   **`Fm` (Fracción Mejorada):**
    Del enunciado: "45% del tiempo instrucciones aritmético-lógicas con enteros" -> `Fm = 0.45`

*   **Aplicar la Ley de Amdahl:**
    $$ A_{\text{ALU}} = \frac{1}{(1 - 0.45) + \frac{0.45}{1.25}} = \frac{1}{0.55 + 0.36} = \frac{1}{0.91} \approx 1.099 $$
    La aceleración total por esta mejora es de aproximadamente **1.099**.

---

### Mejora 2: Coprocesador de Punto Flotante

*   **Descripción:** Las instrucciones en punto flotante se ejecutan al doble de velocidad.
*   **`Am` (Aceleración de la Mejora):**
    Del enunciado: "al doble de velocidad" -> `Am = 2`
*   **`Fm` (Fracción Mejorada):**
    Del enunciado: "10% del tiempo de instrucciones en punto flotante" -> `Fm = 0.10`

*   **Aplicar la Ley de Amdahl:**
    $$ A_{\text{FP}} = \frac{1}{(1 - 0.10) + \frac{0.10}{2}} = \frac{1}{0.90 + 0.05} = \frac{1}{0.95} \approx 1.052 $$
    La aceleración total por esta mejora es de aproximadamente **1.052**.

---

## Notas Relacionadas
*   [[cpi_ciclos_por_instruccion]]
*   [[frecuencia_ciclos_reloj]]

---

## Estrategia para Cálculos sin Calculadora (Método de Fracciones)

En un examen sin calculadora, realizar divisiones con decimales es lento y propenso a errores. El método más robusto es convertir todos los datos a fracciones y operar con ellas.

### Pasos a Seguir
1.  **Convertir Datos:** Transforma todos los porcentajes y decimales a fracciones. (Ej: 45% = 45/100 = 9/20; 0.8 = 8/10 = 4/5).
2.  **Calcular `Am` y `(1-Fm)`:** Usa las fracciones para hallar la aceleración de la mejora y la fracción no mejorada.
3.  **Sustituir en la Fórmula:** Reemplaza los valores en la Ley de Amdahl.
4.  **Resolver el Denominador:**
    a. Primero, resuelve la división `Fm / Am` (multiplicando `Fm` por la inversa de `Am`).
    b. Luego, suma el resultado a `(1-Fm)`, encontrando un denominador común.
5.  **Resultado Final:** La aceleración total `A` será la inversa de la fracción resultante en el denominador. Es perfectamente válido dejar el resultado como una fracción (ej: 100/91).

### Ejemplo Práctico con Fracciones (Mejora de la ALU)

*   **Datos en fracciones:**
    *   `Fm` (Fracción Mejorada) = 45% = **9/20**
    *   `1 - Fm` (Fracción No Mejorada) = 1 - 9/20 = **11/20**
    *   `Am` (Aceleración de la Mejora) = 1 / 0.8 = 1 / (4/5) = **5/4**

*   **Sustituir en la fórmula:**
    $$ A = \frac{1}{\frac{11}{20} + \frac{9/20}{5/4}} $$

*   **Resolver la división `Fm / Am`:**
    $$ \frac{9/20}{5/4} = \frac{9}{20} \times \frac{4}{5} = \frac{36}{100} = \frac{9}{25} $$

*   **Sumar las fracciones del denominador:**
    $$ \frac{11}{20} + \frac{9}{25} $$
    El denominador común es 100:
    $$ \frac{55}{100} + \frac{36}{100} = \frac{91}{100} $$

*   **Calcular el resultado final:**
    $$ A = \frac{1}{91/100} = \frac{100}{91} $$

El resultado exacto es `100/91`, que es aproximadamente `1.099`.
