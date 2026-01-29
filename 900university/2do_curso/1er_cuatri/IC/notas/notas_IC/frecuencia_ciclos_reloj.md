# Frecuencia y Ciclos de Reloj en la CPU

El procesador de un computador opera bajo el ritmo de un reloj interno que sincroniza todas sus operaciones. La comprensión de este ritmo es fundamental para entender el rendimiento de una CPU.

## El Reloj (Clock)

Es el componente físico dentro del procesador que genera una secuencia regular de pulsos eléctricos. Actúa como el "metrónomo" o "corazón" de la CPU, marcando el compás para todas las operaciones.

## Ciclo de Reloj (Clock Cycle)

Cada pulso generado por el reloj es un **ciclo de reloj**. Es la unidad de tiempo más pequeña e indivisible en la que la CPU puede realizar una operación básica. Todas las operaciones del procesador se miden en múltiplos de ciclos de reloj.

*   **Tiempo de Ciclo (Periodo):** Es la duración de un único ciclo de reloj. Se mide en unidades de tiempo (segundos, nanosegundos - `ns`).
    *   **Frase clave:** "Tiene un ciclo de reloj de 1 nanosegundo." (Indica el Tiempo de Ciclo).

## Frecuencia de Reloj (Clock Frequency / Clock Speed)

Es la velocidad a la que el reloj genera ciclos. Indica cuántos ciclos de reloj se producen por segundo.

*   **Medida:** Se expresa en Hertz (Hz).
    *   1 Hz = 1 ciclo por segundo.
    *   1 MHz (Megahercio) = 1 millón de ciclos por segundo.
    *   1 GHz (Gigahercio) = 1.000 millones de ciclos por segundo.
    *   **Frase clave:** "Tiene un reloj de 3 Gigahercios." (Indica la Frecuencia de Reloj).

## Relación Fundamental

La frecuencia de reloj y el tiempo de ciclo son **inversamente proporcionales**:

$$ \text{Tiempo de Ciclo} = \frac{1}{\text{Frecuencia de Reloj}} $$

$$ \text{Frecuencia de Reloj} = \frac{1}{\text{Tiempo de Ciclo}} $$

**Ejemplo:**
*   Si una CPU tiene una Frecuencia de Reloj de **2 GHz** (`2 \times 10^9 \text{ Hz}`).
*   Su Tiempo de Ciclo será `1 / (2 \times 10^9) = 0.5 \times 10^{-9} \text{ segundos} = 0.5 \text{ ns}`.

Esto significa que un ciclo de reloj de esta CPU dura medio nanosegundo.

---
## Notas Relacionadas
*   [[cpi_ciclos_por_instruccion]] (El CPI se calcula usando los ciclos de reloj).
