
# 20260112174500 Guía Paso a Paso para Álgebra Relacional

**Tipo:** #guia #metodologia
**Área:** #bases_de_datos #modelo_relacional
**Fuente:** `t1-Modelo_Relacional/BD.1.MR.AR.pdf`, `t1-Modelo_Relacional/BD.1.MR.AR.Ej.pdf.pdf`

---

## Objetivo

Esta guía establece una metodología clara y generalizable para resolver ejercicios de álgebra relacional, traduciendo una petición en lenguaje natural a una expresión formal del álgebra.

## Metodología Paso a Paso

El enfoque general es construir la consulta desde adentro hacia afuera, empezando por las restricciones más específicas y terminando con la proyección de los datos deseados.

### Paso 1: Descomponer la Consulta

Lee atentamente la petición e identifica las partes clave:
1.  **¿Qué datos se piden al final?** (Esto definirá la Proyección final - `$\pi$`).
2.  **¿Qué condiciones deben cumplir los datos?** (Esto se traduce en Selecciones - `$\sigma$`).
3.  **¿De qué tablas (relaciones) proviene la información?** (Esto indica las relaciones base y los Joins - `$\bowtie$` - necesarios para combinarlas).

### Paso 2: Identificar y Conectar las Relaciones de Origen

-   Haz una lista de las entidades que contienen los datos que necesitas.
-   Si la información está en más de una tabla, necesitas combinarlas. La operación principal para esto es el **Natural Join (`$\bowtie$`)**.
-   El `Natural Join` combina automáticamente las filas de dos relaciones que tienen el mismo valor en los atributos con el mismo nombre. Es la forma más común y eficiente de reunir información relacionada.

*Ejemplo:* Si pides el nombre de un empleado y el nombre de su departamento, necesitas `Empleado` y `Departamento`. Las combinarías con `Empleado $\bowtie$ Departamento`.

### Paso 3: Aplicar Condiciones (Selección - σ)

-   Por cada condición identificada en el Paso 1, aplica un operador de Selección (`$\sigma$`).
-   La selección filtra las filas de una relación, manteniendo solo aquellas que cumplen la condición.
-   **Estrategia:** Aplica las selecciones tan pronto como sea posible. Filtrar datos al principio reduce el tamaño de las relaciones intermedias, haciendo las operaciones siguientes (como los joins) más eficientes.

*Ejemplo:* Para "proyectos del año 2023", aplicarías $\sigma_{año=2023}(Proyecto)$.

### Paso 4: Construir la Expresión Anidada

-   Combina los pasos 2 y 3. Empieza por las selecciones más internas y ve "envolviéndolas" con los joins.
-   Piensa en la secuencia de operaciones. A menudo, el patrón es: `Join(Selección(Tabla1), Selección(Tabla2))`.

*Ejemplo:* "Nombre de los voluntarios de 'A Coruña' que participan en proyectos iniciados en 2015".
1.  **Relaciones:** `Voluntario`, `Participa`, `Proyecto`.
2.  **Condiciones:** `localidad='A Coruña'` en `Voluntario`, `añoInicio=2015` en `Proyecto`.
3.  **Combinación:**
    -   Primero, filtramos voluntarios: `$\sigma_{localidad='A Coruña'}(Voluntario)$`
    -   Luego, filtramos proyectos: `$\sigma_{añoInicio=2015}(Proyecto)$`
    -   Ahora, unimos todo: `$$(\sigma_{localidad='A Coruña'}(Voluntario)) \bowtie Participa \bowtie (\sigma_{añoInicio=2015}(Proyecto))$$`

### Paso 5: Proyectar los Atributos Finales (Proyección - π)

-   Una vez que tienes la relación final con todas las filas correctas, usa el operador de Proyección (`$\pi$`) para quedarte únicamente con las columnas (atributos) que pide el enunciado.
-   Este es el último paso, el más externo de tu expresión.

*Ejemplo (continuación):*
-   $$\pi_{nombrePila, apellidos}( (\sigma_{localidad='A Coruña'}(Voluntario)) \bowtie Participa \bowtie (\sigma_{añoInicio=2015}(Proyecto)) )$$

## Manejo de Casos Especiales

### Consultas "Para Todos": Operador de División (`$\div$`)

-   **Cuándo usarlo:** Cuando la consulta contiene palabras como "todos", "siempre", "únicamente". Por ejemplo, "empleados que trabajan en *todos* los proyectos" o "proveedores que suministran *todas* las piezas".
-   **Intuición:** La división `$A \div B$` responde a la pregunta: "¿Qué miembros del conjunto `A` están relacionados con *todos* los miembros del conjunto `B`?".
-   **Estructura:**
    1.  **Dividendo (A):** Debe ser una relación con los atributos que quieres en el resultado MÁS los atributos con los que comparas. Típicamente es una tabla de "participación" o una combinación. Ej: `$\pi_{id_empleado, id_proyecto}(Participa)$`.
    2.  **Divisor (B):** Es la relación que contiene el conjunto de "todos" los elementos. Ej: `$\pi_{id_proyecto}(Proyecto)$`.
    3.  **Resultado:** El resultado tendrá los atributos del dividendo menos los del divisor. Ej: `id_empleado`.

*Ejemplo:* "Voluntarios (nombre) que participan en TODOS los proyectos registrados".
1.  **Dividendo:** Necesitamos la relación de quién participa en qué. `$\pi_{idVoluntario, idProyecto}(Participa)$`.
2.  **Divisor:** Necesitamos el conjunto de "todos los proyectos". `$\pi_{idProyecto}(Proyecto)$`.
3.  **División:** `$\pi_{idVoluntario, idProyecto}(Participa) \div \pi_{idProyecto}(Proyecto)$`. Esto nos da los `idVoluntario`.
4.  **Obtener nombre:** Hacemos un join con `Voluntario` y proyectamos el nombre.
    $$\pi_{nombrePila, apellidos}( (\pi_{idVoluntario, idProyecto}(Participa) \div \pi_{idProyecto}(Proyecto)) \bowtie Voluntario )$$

### Operaciones de Conjuntos: Unión (`$\cup$`), Intersección (`$\cap$`), Diferencia (`-`)

-   **Requisito:** Las dos relaciones deben ser **unión-compatibles** (mismo número de atributos y dominios compatibles en el mismo orden).
-   **Unión (`$\cup$`):** Para consultas con "o". Devuelve las filas que están en la primera relación, en la segunda, o en ambas.
-   **Intersección (`$\cap$`):** Para consultas con "y" a nivel de conjuntos. Devuelve las filas que están en ambas relaciones.
-   **Diferencia (`-`):** Para consultas con "pero no" o "excepto". Devuelve las filas que están en la primera relación pero NO en la segunda.

### Renombrar (`$\rho$` o `$\leftarrow$`)

-   Esencial cuando se hacen `self-joins` (unir una tabla consigo misma) para distinguir entre atributos con el mismo nombre.
-   También útil para dar nombres a las relaciones intermedias y hacer la expresión más legible.

## Resumen de la Estrategia

1.  **IDENTIFICA** lo que se pide, las condiciones y las tablas.
2.  **FILTRA** primero con `$\sigma$` (Selección) lo más cerca posible de las tablas originales.
3.  **COMBINA** después con `$\bowtie$` (Join) las relaciones filtradas.
4.  **MANEJA** casos especiales como la `$\div$` (División) si se pide "todos".
5.  **PROYECTA** al final con `$\pi$` (Proyección) solo los campos que quieres mostrar.

---
## Conexiones
-   [[20260109130510-dominio-en-modelo-relacional]]
-   El álgebra relacional es la base formal para el lenguaje de consulta SQL.
