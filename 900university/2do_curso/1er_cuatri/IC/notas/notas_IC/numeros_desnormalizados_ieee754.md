# Números Desnormalizados (Subnormales) en IEEE 754

Un número en formato IEEE 754 se considera **desnormalizado** (o subnormal) bajo condiciones muy específicas, diseñadas para manejar números extremadamente pequeños de forma gradual.

## Definición a Nivel de Bits

Un número es desnormalizado si su representación binaria cumple que:
*   El campo del **exponente es todo ceros** (`00...0`).
*   El campo de la **mantisa es distinto de cero**.

Si tanto el exponente como la mantisa son todo ceros, el número es **cero**, no desnormalizado.

## ¿Cómo Saber si un Número será Desnormalizado?

La regla práctica es: un número (en valor absoluto) se representará como desnormalizado si es **más pequeño que el número normalizado más pequeño** que se puede representar en ese formato.

1.  **Calcular el Límite:**
    *   Para ser **normalizado**, el exponente almacenado mínimo es `1`.
    *   En precisión simple (sesgo=127), el exponente real mínimo es `1 - 127 = -126`.
    *   La mantisa normalizada más pequeña (con el bit implícito) es `1.0`.
    *   Por tanto, el número normalizado positivo más pequeño es `1.0 * 2^(-126)`.

2.  **La Prueba:**
    Cualquier número con una magnitud **menor que `1.0 * 2^(-126)`** deberá ser representado como desnormalizado. Al intentar convertirlo, el exponente real que se necesita será menor a -126, lo que resultará en un exponente almacenado menor a 1. Este es el indicador para usar la representación desnormalizada.

    `Exponente_Almacenado = Exponente_Real + Sesgo`
    Si `Exponente_Almacenado < 1`, es un caso desnormalizado.

## Propósito: "Underflow Gradual"

Los números desnormalizados llenan el "hueco" que existiría entre el cero y el número normalizado más pequeño. Esto permite que los cálculos que producen resultados muy pequeños no se redondeen bruscamente a cero, sino que pierdan precisión de forma gradual. Esto se conoce como **"underflow gradual"**.

## Cálculo del Valor Desnormalizado

Cuando un número es desnormalizado, la fórmula para calcular su valor cambia:

**Valor = `(-1)^S * 0.M * 2^(1 - Bias)`**

Hay dos diferencias cruciales con la fórmula normalizada:
1.  **Bit implícito `0`**: Se asume un `0.` delante de la mantisa (`M`), en lugar de un `1.`.
2.  **Exponente Fijo**: El exponente real se fija al valor del exponente normalizado más pequeño, es decir, `1 - Bias` (ej: -126 para precisión simple).

### Ejemplo:
Si al normalizar un número, obtenemos que el exponente real necesario es `-128`, el exponente a almacenar sería `-128 + 127 = -1`, lo cual es imposible. Esto nos obliga a usar la representación desnormalizada.

El valor del número se reinterpreta para ajustarse a la fórmula desnormalizada:
`Valor_Original = 0.M * 2^(-126)`
Se despeja `0.M` y esa es la mantisa que se almacena en el campo correspondiente.

---
## Notas Relacionadas
*   [[sesgo_exponente_ieee754]]
