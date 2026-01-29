---
id: 20260109130510
title: Dominio en el Modelo Relacional
tags:
- bases-de-datos
- modelo-relacional
- dominio
---

# Dominio en el Modelo Relacional

## Distinción Fundamental: Dos Usos del Término 'Dominio'

Es crucial entender que la palabra "dominio" se utiliza en bases de datos con dos significados distintos, aunque relacionados:

1.  **Dominio Contextual (o Minimundo / Dominio del Discurso):**
    *   Se refiere al **ámbito o porción del mundo real** que una base de datos intenta modelar. Es el universo de interés del sistema.
    *   **Ejemplo:** En un sistema para una universidad, el "Dominio Contextual" sería la propia universidad (la UDC, sus estudiantes, profesores, asignaturas, etc.). Este es el sentido en que se usa en las diapositivas introductorias (p. ej., diapositivas 5 y 6 del PDF).

2.  **Dominio de Atributo:**
    *   Este es el sentido **técnico y formal** dentro del modelo relacional. Se refiere al conjunto de todos los valores **atómicos** (indivisibles) permitidos para un [[20260109130520-atributo-modelo-relacional|atributo]] específico (columna) en una tabla.

Esta nota se centra principalmente en el **Dominio de Atributo**, que es fundamental para la estructura y la integridad del modelo relacional.

---

En el contexto del **modelo relacional**, un **dominio de atributo** se define como el conjunto de todos los valores **atómicos** (indivisibles) permitidos para un [[20260109130520-atributo-modelo-relacional|atributo]] específico.

Funciona como una restricción que asegura la integridad de los datos, garantizando que cada valor en una columna sea válido.

### Características Clave del Dominio de Atributo
-   **Ligado al Atributo:** Cada atributo (columna) de una tabla está asociado a un único dominio de atributo.
-   **Reutilizable:** Varios atributos, incluso de tablas diferentes, pueden compartir el mismo dominio de atributo.
-   **Analogía:** Es similar al **dominio de una función matemática** (el conjunto de entradas válidas) o al **tipo de dato** de una variable en programación.

### Definición de Dominio de Atributo
Un dominio de atributo se puede especificar de dos maneras:
1.  **Por Extensión:** Listando explícitamente todos los valores permitidos.
    -   **Ejemplo:** `dom(TipoMateria) = {"Obligatoria", "Optativa", "Libre"}`
2.  **Por Comprensión:** Mediante una regla o condición que los valores deben cumplir.
    -   **Ejemplo:** `dom(Edad) = {x | x es un entero y 0 <= x < 120}`
    -   **Ejemplo:** `dom(DNI) = {x | x es una cadena que cumple el formato de DNI válido}`

### Ejemplo de Uso Compartido del Dominio de Atributo
Se puede definir un dominio de atributo `PuntuacionNumerica` como los enteros entre 0 y 100. Luego, los atributos `ScoreParcial` y `ScoreFinal` pueden ambos usar este mismo dominio, asegurando consistencia. Otro ejemplo es un dominio para "números enteros mayores que -1 y menores a 161", que podría ser utilizado por atributos como `Edad` y `ScoreEnExamen`.