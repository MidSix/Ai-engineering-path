# CPI (Ciclos Por Instrucción)

El CPI (Cycles Per Instruction - Ciclos Por Instrucción) es una métrica fundamental para evaluar la eficiencia con la que un procesador ejecuta las instrucciones de un programa. Representa el número promedio de ciclos de reloj que el procesador necesita para completar cada instrucción.

## Fórmula Básica (CPI Promedio)

La definición más directa del CPI es:

$$ \text{CPI}_{\text{promedio}} = \frac{\text{Número total de ciclos de reloj}}{\text{Número total de instrucciones ejecutadas}} $$

>**Aclaración Importante:** Esta fórmula calcula el **CPI promedio** para una ejecución de programa específica. **No asume** que todas las instrucciones tardan el mismo número de ciclos. De hecho, su utilidad radica precisamente en que promedia el coste de una mezcla de instrucciones rápidas y lentas para dar una métrica de rendimiento global para esa mezcla concreta.

## ¿Qué es un Promedio Ponderado?

Antes de ver la segunda forma de calcular el CPI, es crucial entender qué es un promedio ponderado.

Un **promedio simple** (el que solemos usar) trata a todos los elementos por igual. Si calculas el promedio de `(8, 9, 10)`, el resultado es `(8+9+10)/3 = 9`.

Un **promedio ponderado**, en cambio, da más importancia (o "peso") a unos elementos que a otros. Cada elemento se multiplica por su "peso" (su importancia relativa) y luego se suman los resultados.

La fórmula general es:
`Promedio Ponderado = (Valor_A × Peso_A) + (Valor_B × Peso_B) + ...`

La suma de todos los pesos debe ser 1 (o 100%), es decir trabajamos en una proporcion 0-1.

### Ejemplo Sencillo: Cálculo de Nota Final

Imagina que la nota de una asignatura se calcula así:
*   El examen final cuenta un **60%** (peso = 0.60).
*   Los trabajos prácticos cuentan un **30%** (peso = 0.30).
*   La asistencia cuenta un **10%** (peso = 0.10).

Un alumno obtiene las siguientes notas:
*   Nota de examen: **7**
*   Nota de trabajos: **10**
*   Nota de asistencia: **9**

Un promedio simple sería `(7+10+9)/3 = 8.67`, lo cual es incorrecto porque no considera la importancia de cada parte.

El **cálculo correcto (promedio ponderado)** es:
`Nota Final = (7 × 0.60) + (10 × 0.30) + (9 × 0.10)`
`Nota Final = 4.2 + 3.0 + 0.9 = 8.1`

La nota final es 8.1. Es más baja que el promedio simple porque la nota del examen (7), que era la más baja, tenía el peso más alto (60%).

## CPI como Promedio Ponderado

Esta es exactamente la lógica que se usa para la segunda fórmula del CPI. No todas las instrucciones "pesan" lo mismo: unas son más frecuentes que otras.

$$ \text{CPI} = \sum_{i=1}^{n} (\text{CPI}_i \times \text{Frecuencia}_i) $$

Aquí, la analogía es:
*   Los **Valores** son los `CPI_i`: el coste en ciclos de cada tipo de instrucción (ej: ALU=1 ciclo, Load=3 ciclos, Div=10 ciclos).
*   Los **Pesos** son las `Frecuencia_i`: el porcentaje de veces que aparece cada tipo de instrucción en el programa.

Esta fórmula permite calcular el CPI promedio de un programa sin necesidad de ejecutarlo, siempre que se conozca la frecuencia de sus instrucciones y el coste de cada una.

## Significado
Un valor de CPI más bajo indica un procesador más eficiente, ya que necesita menos ciclos de reloj, en promedio, para completar cada instrucción.

---
## Notas Relacionadas
*   [[frecuencia_ciclos_reloj]]
*   [[representacion_direcciones_vs_valores]]
