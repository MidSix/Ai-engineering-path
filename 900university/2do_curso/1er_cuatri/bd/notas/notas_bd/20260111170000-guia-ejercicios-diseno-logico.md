# 20260111170000 Guía Práctica para Ejercicios de Diseño Lógico (Normalización)

**Etiquetas:** #bases_de_datos #diseño_lógico #normalización #guía_práctica #ejercicios #FNBC

Esta nota resume los pasos y conceptos mínimos para resolver un ejercicio típico de diseño lógico ascendente, donde se te da un escenario y debes llegar a un conjunto de tablas normalizadas.

---

### El Objetivo: Descomponer para Eliminar Anomalías

Recuerda el porqué: se busca descomponer tablas grandes y mal diseñadas en tablas más pequeñas y bien estructuradas para eliminar la redundancia y las anomalías de inserción, actualización y borrado. El objetivo final suele ser que todas las tablas estén en **FNBC (Forma Normal de Boyce-Codd)**.

---

### Pasos para Resolver un Ejercicio de Normalización

**Dado:** Un enunciado en lenguaje natural que describe una situación (ej: un centro deportivo, una biblioteca, etc.).

#### Paso 1: Identificar las Dependencias Funcionales (DFs) a partir del Enunciado

Este es el verdadero primer paso. Consiste en "traducir" las reglas del negocio descritas en el texto a dependencias funcionales formales (`X -> Y`).

**Busca estas Pistas y Palabras Clave:**

1.  **Unicidad e Identificadores:**
    *   "**Cada** empleado tiene **un único** DNI": `ID_Empleado -> DNI` (y también `DNI -> ID_Empleado`).
    *   "El código de la asignatura es **único**": `CodAsignatura` probablemente sea una clave candidata de la tabla de asignaturas. Implica una DF `CodAsignatura -> {NombreAsig, Creditos, ...}`.
    *   "**No hay dos** productos con el mismo nombre": `NombreProducto -> ID_Producto` (o es una clave candidata).

2.  **Determinación Directa:**
    *   "**Para cada** libro, conocemos **su** autor y editorial": `ID_Libro -> {Autor, Editorial}`.
    *   "**De cada** socio se guarda **su** nombre y dirección": `ID_Socio -> {Nombre, Direccion}`.
    *   "El importe de la multa **depende del** tipo de infracción": `TipoInfraccion -> ImporteMulta`.

3.  **Relaciones "Uno a Muchos" vs. "Muchos a Muchos" (¡CUIDADO!):**
    *   "Un empleado trabaja en **un solo** departamento": ¡Esto **SÍ** es una DF! `ID_Empleado -> ID_Departamento`.
    *   "Un empleado puede trabajar en **varios** proyectos": ¡Esto **NO** es una DF del tipo `ID_Empleado -> ID_Proyecto`! Esto indica una relación muchos a muchos. La DF real estará en la tabla que une a ambos, y la clave será compuesta: `{ID_Empleado, ID_Proyecto} -> {Horas, FechaInicio, ...}`.
    *   **La clave es fijarse en "un/uno/una" (implica DF) frente a "varios/muchos" (implican una relación multivaluada, no una DF simple).**

**Proceso Sugerido:**
1.  Lee el enunciado e identifica los atributos (la información que se quiere guardar).
2.  Busca los atributos que son **identificadores** (códigos, IDs, DNI). Estos serán la parte izquierda (`X`) de muchas DFs.
3.  Para cada atributo, pregúntate: "¿Su valor se determina conociendo el valor de otro atributo?". Usa las pistas de arriba.
4.  Escribe la lista completa de DFs que has encontrado. Este es tu conjunto `F`.

#### Paso 2: Calcular TODAS las Claves Candidatas (CC)

Este es el paso **más importante y el segundo que debes hacer**. Sin las claves, no puedes determinar las formas normales.

1.  **Herramienta: Cierre de un conjunto de atributos (X+):** Para un conjunto de atributos `X`, su cierre `X+` son todos los atributos que `X` puede determinar.
    *   **Algoritmo:** Empieza con `X+ = X`. Recorre las DFs en `F`. Si tienes una DF `A -> B` y `A` ya está en `X+`, añade `B` a `X+`. Repite hasta que no puedas añadir más atributos.
2.  **Encontrar Superclaves:** Un conjunto de atributos `X` es una **superclave** si su cierre `X+` contiene TODOS los atributos de la relación `R`.
3.  **Encontrar Claves Candidatas:** Una **clave candidata** es una superclave **mínima**. Es decir, si le quitas cualquier atributo, deja de ser una superclave.

#### Paso 3: Determinar la Forma Normal de la Relación

*   **Regla de Oro de la FNBC:** Para **toda** dependencia funcional `X -> Y` que se cumpla en la tabla (y que no sea trivial), ¿es `X` una **superclave**?
    *   **Si la respuesta es SÍ** para todas las DFs, la tabla **está en FNBC**.
    *   **Si encuentras UNA SOLA DF `X -> Y` donde `X` NO es superclave**, la tabla **NO está en FNBC** y debes descomponerla.

#### Paso 4: Descomponer la Relación (si no está en FNBC)

1.  **Elige una DF "culpable":** Escoge una DF `X -> Y` que viole la FNBC.
2.  **Crea dos tablas nuevas:**
    *   **Tabla 1:** `R1(X, Y)` - Contiene los atributos de la DF culpable.
    *   **Tabla 2:** `R2(R - Y)` - Contiene el resto de atributos de la tabla original.
3.  **Propiedades:** Esta descomposición garantiza que no hay **pérdida de información**.

#### Paso 5: Repetir el Proceso

Ahora tienes un nuevo conjunto de tablas (`R1`, `R2`, ...). Debes **analizar cada una de ellas por separado**, volviendo al Paso 2: calcula sus DFs, encuentra sus claves y comprueba si están en FNBC. Si alguna no lo está, la descompones de nuevo.

#### Paso 6: Solución Final

La solución es el conjunto final de esquemas de relación, cada una con sus atributos, sus DFs y su clave primaria, y todas en FNBC.

---
**Enlaces:**
- [[20260111163000-df-semantica-diseno-logico-ascendente]]
- [[20260111160000-modelizacion-temporal-de-datos]]
