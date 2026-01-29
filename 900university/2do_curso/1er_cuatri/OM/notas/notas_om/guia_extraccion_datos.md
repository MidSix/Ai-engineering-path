# Guía de Extracción de Datos y Modelado en Optimización Matemática

Esta guía establece un *pipeline* paso a paso para transformar un enunciado en lenguaje natural a un modelo matemático formal.

## Modelo Mental Clave: Las 4 Constantes de la Modelización
Casi todo problema de programación lineal tiene estos 4 componentes fundamentales:

1.  **Los Recursos Limitados:** Aquello que tenemos o poseemos y se agota (Horas de CPU, Dinero, Materia prima).
2.  **Las Actividades:** Los elementos que consumen o usan nuestros recursos (Tareas, Producción de bienes, Rutas).
3.  **Las Variables de Decisión ($x_i$):** ¿**CUÁNTAS** de estas actividades (que consumen recursos) debo realizar?
    *   *Regla de Oro:* No es el recurso ni la actividad abstracta, es la **cantidad** de actividad.
4.  **El Objetivo:** Maximizar o minimizar el resultado asociado a realizar dichas actividades (Beneficio generado, Coste incurrido).

## Coeficientes Tecnológicos: La Tasa de Conversión
¿Qué significan los números que multiplican a las variables (ej. $1.5x_3$)?
No son "veces que se hace la actividad". Son una **tasa de conversión** o equivalencia entre lo que produces y lo que gastas.

*   **Fórmula:** `(Impacto por unidad) * (Cantidad producida) = Impacto Total`
*   **Análisis Dimensional:** Es la clave para no equivocarse.
    $$ \left( \frac{\text{[Recurso]}}{\text{[Unidad de Actividad]}} \right) \cdot (\text{Unidades de Actividad}) = \text{Recurso Total} $$
    *   *Ejemplo:* Si produces Energía ($x_3$ en MWh) y gastas Agua ($m^3$):
    $$ 1.5 \frac{m^3}{MWh} \cdot x_3 (MWh) = 1.5 x_3 (m^3) $$

## El Pipeline de Modelado

**Entrada:** Enunciado del problema (texto).
**Salida:** Modelo matemático (Variables, F.O., Restricciones).

### Paso 1: Lectura de Reconocimiento ("El Escáner")
**Objetivo:** Identificar el contexto y el "rol" que juegas.
*   Lee el enunciado completo rápidamente.
*   Pregúntate: ¿Qué soy? (¿Gerente de fábrica? ¿Dietista? ¿Planificador de rutas?).
*   Identifica la "pregunta final": Suele estar al final del texto ("determinar...", "decidir...", "planificar...").

### Paso 2: Definición de Variables de Decisión (El "¿Qué controlo?")
**Objetivo:** Identificar qué valores numéricos debes determinar.
*   **Busca:** Sustantivos cuantificables asociados a la pregunta final.
*   **Convención:**
    *   $x_i$, $x_{ij}$, etc.
*   **CRÍTICO: Define las UNIDADES.**
    *   ¿Son "unidades producidas"? ¿"horas asignadas"? ¿"kg de mezcla"? ¿"número de camiones"?
    *   *Ejemplo:* $x_1$: Cantidad de MWh producidos en la planta Solar.

### Paso 3: Identificación del Objetivo (El "¿Qué quiero lograr?")
**Objetivo:** Definir la meta y la dirección de la optimización.
*   **Busca:**
    *   "Maximizar beneficio/rentabilidad/ingresos/producción". -> **MAX**
    *   "Minimizar coste/tiempo/distancia/desperdicio/grasa". -> **MIN**
*   **Coeficientes:** Identifica el valor unitario asociado a cada variable en el objetivo (e.g., precio de venta, coste por hora).
*   **Formato:** Max/Min $Z = c_1x_1 + c_2x_2 + ...$

### Paso 4: Extracción de Restricciones (El "¿Qué me limita?")
**Objetivo:** Traducir las limitaciones del mundo real a desigualdades.
Este es el paso más denso. Busca palabras clave para identificar el tipo de restricción:

| Tipo | Palabras Clave | Signo | Ejemplo |
| :--- | :--- | :--- | :--- |
| **Capacidad** | "disponible", "máximo", "tiene un stock de", "no puede superar" | $\le$ | Horas máquina, presupuesto, stock mp. |
| **Requerimiento** | "al menos", "mínimo", "debe cubrir", "necesita garantizar" | $\ge$ | Demanda, nutrientes, contratos. |
| **Balance/Definición** | "es igual a", "la proporción es", "exactamente" | $=$ | Mezclas, flujos de entrada/salida. |
| **Proporción** | "el doble de X que de Y", "el 30% del total" | $\le, \ge, =$ | $x_1 \ge 2x_2$, $x_1 \le 0.3(x_1+x_2)$. |

*   **Truco:** Para cada restricción, pregúntate: *¿En qué unidades se mide esta restricción?* (e.g., Si la restricción es de "horas de máquina", todos los términos de la ecuación deben ser horas).

### Paso 5: Naturaleza de las Variables
*   **No negatividad:** Casi siempre $x_i \ge 0$. (No puedes producir -5 sillas).
*   **Enteras:** ¿Son cosas indivisibles? (Personas, aviones, barcos) -> $x_i \in \mathbb{Z}$.
*   **Binarias:** ¿Son decisiones de Sí/No? (Construir fábrica o no, elegir ruta o no) -> $x_i \in \{0, 1\}$.

---

## Patrones Comunes en los Enunciados (Boletín 1)

Basado en el análisis de `t1/boletin1.pdf`, aquí tienes patrones específicos:

### 1. Problemas de Producción/Recursos (Ej. 1, 2, 8)
*   **Estructura:** Tabla con "Consumo unitario" y "Disponibilidad total".
*   **Variables:** Cantidad a producir ($x_j$).
*   **Restricciones:** $\sum (\text{consumo}_{ij} \cdot x_j) \le \text{Capacidad}_i$.
    *   Fila de la tabla $\rightarrow$ Una ecuación de restricción.

### 2. Problema de la Dieta/Mezcla (Ej. 4)
*   **Estructura:** Tabla con "Aporte nutricional unitario" y "Requerimiento mínimo".
*   **Variables:** Cantidad de ingrediente ($x_j$).
*   **Restricciones:** $\sum (\text{aporte}_{ij} \cdot x_j) \ge \text{Requerimiento}_i$.

### 3. Problema de Transporte/Asignación (Ej. 3)
*   **Estructura:** Matriz de Origen $\rightarrow$ Destino.
*   **Variables:** $x_{ij}$ (Cantidad enviada de $i$ a $j$).
*   **Restricciones:**
    *   Oferta (Salida): $\sum_j x_{ij} \le \text{Oferta}_i$ (Suma horizontal).
    *   Demanda (Llegada): $\sum_i x_{ij} \ge \text{Demanda}_j$ (Suma vertical).

---

## Lista de Verificación (Checklist) Final

1.  [ ] ¿He definido todas las variables claramente y con sus unidades?
2.  [ ] ¿He identificado si es Max o Min?
3.  [ ] ¿Todas las restricciones tienen las mismas unidades a la izquierda y derecha del signo?
4.  [ ] ¿He considerado la no negatividad?
5.  [ ] ¿He comprobado si las variables deben ser enteras?
