# Ejemplo: Cálculo de Mezcla de Instrucciones (Ejercicio 9, Boletín 1)

Este es un ejemplo práctico de cómo utilizar las métricas de rendimiento de la CPU (CPI, frecuencia, tiempo) para determinar la composición de un programa desconocido, basándose en una relación de rendimiento conocida entre dos máquinas diferentes.

## Objetivo del Problema

Dadas dos máquinas (M1, M2) con diferentes características de CPI y frecuencia, y sabiendo que M2 es 4/3 veces más rápida que M1 ejecutando un programa `benchmark`, el objetivo es encontrar la fracción de instrucciones de tipo A y tipo B que componen dicho programa.

## Datos del Enunciado

*   **Máquina M1:**
    *   Frecuencia: 300 MHz
    *   CPI para instrucción tipo A: 1
    *   CPI para instrucción tipo B: 2
*   **Máquina M2:**
    *   Frecuencia: 400 MHz
    *   CPI para instrucción tipo A: 3
    *   CPI para instrucción tipo B: 1
*   **Relación de Rendimiento:**
    *   M2 es 4/3 más rápido que M1, lo que implica: `Tiempo_M1 / Tiempo_M2 = 4/3`.

## Fórmulas Clave

1.  **Tiempo de Ejecución de la CPU:**
    $$ \text{Tiempo} = \frac{\text{Nº de Instrucciones} \times \text{CPI}}{\text{Frecuencia de Reloj}} $$
2.  **CPI Promedio Ponderado:**
    $$ \text{CPI}_{\text{promedio}} = (\text{Fracción}_A \times \text{CPI}_A) + (\text{Fracción}_B \times \text{CPI}_B) $$

## Resolución Paso a Paso

### 1. Definir la Incógnita

Sea **`x`** la fracción de instrucciones de tipo A en el programa. Por lo tanto, la fracción de instrucciones de tipo B será **`(1 - x)`**.

### 2. Calcular el CPI de cada Máquina en Función de `x`

*   **CPI para M1:**
    `CPI_M1 = (x * 1) + ((1-x) * 2) = x + 2 - 2x = 2 - x`
*   **CPI para M2:**
    `CPI_M2 = (x * 3) + ((1-x) * 1) = 3x + 1 - x = 2x + 1`

### 3. Plantear la Ecuación de Tiempos

Usamos la relación de rendimiento conocida:
$$ \frac{\text{Tiempo}_{\text{M1}}}{\text{Tiempo}_{\text{M2}}} = \frac{4}{3} $$
Sustituimos la fórmula del tiempo de ejecución para cada máquina (asumiendo `N` como el número total de instrucciones):
$$ \frac{\frac{N \times \text{CPI}_{\text{M1}}}{\text{Frecuencia}_{\text{M1}}}}{\frac{N \times \text{CPI}_{\text{M2}}}{\text{Frecuencia}_{\text{M2}}}} = \frac{4}{3} $$
El término `N` se cancela. Reorganizando la fracción compleja, obtenemos:
$$ \frac{\text{CPI}_{\text{M1}} \times \text{Frecuencia}_{\text{M2}}}{\text{CPI}_{\text{M2}} \times \text{Frecuencia}_{\text{M1}}} = \frac{4}{3} $$

### 4. Sustituir los Datos y Resolver para `x`

$$ \frac{(2-x) \times 400 \text{ MHz}}{(2x+1) \times 300 \text{ MHz}} = \frac{4}{3} $$
Simplificamos `400/300` que es `4/3`:
$$ \frac{(2-x) \times 4}{(2x+1) \times 3} = \frac{4}{3} $$
Podemos cancelar el término `4/3` en ambos lados de la ecuación:
$$ \frac{2-x}{2x+1} = 1 $$
$$ 2 - x = 2x + 1 $$
$$ 1 = 3x $$
$$ x = \frac{1}{3} $$

### 5. Conclusión

La fracción de instrucciones de tipo A en el programa benchmark es **1/3**, lo que corresponde a un **33.3%**. Consecuentemente, las instrucciones de tipo B componen el **66.7%** restante del programa.

---
## Notas Relacionadas
*   [[cpi_ciclos_por_instruccion]]
*   [[frecuencia_ciclos_reloj]]
*   [[ley_de_amdahl]]
