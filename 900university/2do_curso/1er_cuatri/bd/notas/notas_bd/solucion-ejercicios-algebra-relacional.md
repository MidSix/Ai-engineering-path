
# Soluciones a los Ejercicios de Álgebra Relacional

## Orden de Ejecución de las Cláusulas SQL

1.  **`FROM`**: Se parte de una o más tablas para obtener una tabla combinada.
2.  **`WHERE`**: Se eliminan las filas que no cumplen la condición especificada.
3.  **`GROUP BY`**: Se agrupan las filas que comparten un valor en una o más columnas.
4.  **`HAVING`**: Se eliminan los grupos que no cumplen la condición especificada.
5.  **`SELECT`**: Se procesan las filas/grupos resultantes para generar las columnas del resultado final. Se aplica `DISTINCT` si se especifica.
6.  **`ORDER BY`**: Se ordenan las filas del resultado final.

---

## Ejercicios Bloque 1

**Esquema:**
-   `Voluntario (idVoluntario, nombrePila, apellidos, localidad)`
-   `Proyecto (idProyecto, nombre, añoInicio, presupuesto)`
-   `Participa (idVoluntario, idProyecto)`
-   `Informe (idInforme, fecha, titulo, texto, idVoluntarioAutor, idProyecto)`

### 1. Traduce a SQL

> $\pi_{idVoluntario}(Participa)$

**SQL:**
```sql
SELECT DISTINCT idVoluntario
FROM Participa;
```

---

### 2. Consultas

#### a) Obtener el presupuesto del proyecto con identificador 'P001'.

**Álgebra Relacional:**

$\pi_{presupuesto}(\sigma_{idProyecto='P001'}(Proyecto))$


**SQL:**
```sql
SELECT presupuesto
FROM Proyecto
WHERE idProyecto = 'P001';
```

#### b) Obtener el identificador y nombre de los proyectos iniciados en 2016 y con un presupuesto superior a 10.000 euros.

**Álgebra Relacional:**

$\pi_{idProyecto, nombre}(\sigma_{añoInicio=2016 \text{ AND } presupuesto > 10000}(Proyecto))$


**SQL:**
```sql
SELECT idProyecto, nombre
FROM Proyecto
WHERE añoInicio = 2016 AND presupuesto > 10000;
```

#### c) Obtener el título y fecha de todos los informes redactados por voluntarios de 'A Coruña'.

**Álgebra Relacional:**

$$\pi_{titulo, fecha}(Informe \bowtie \sigma_{localidad='A Coruña'}(\rho_{idVoluntarioAutor \leftarrow idVoluntario}(Voluntario)))$$

*(Nota: Renombramos `idVoluntario` a `idVoluntarioAutor` para que el Natural Join funcione correctamente)*

**SQL:**
```sql
SELECT I.titulo, I.fecha
FROM Informe I
JOIN Voluntario V ON I.idVoluntarioAutor = V.idVoluntario
WHERE V.localidad = 'A Coruña';
```

#### d) Obtener el nombre de pila y apellidos de todos los voluntarios que participan en el proyecto con identificador 'P001'.

**Álgebra Relacional:**

$$\pi_{nombrePila, apellidos}((\sigma_{idProyecto='P001'}(Participa)) \bowtie Voluntario)$$


**SQL:**
```sql
SELECT V.nombrePila, V.apellidos
FROM Voluntario V
JOIN Participa P ON V.idVoluntario = P.idVoluntario
WHERE P.idProyecto = 'P001';
```

#### e) Obtener el nombre y apellidos de aquellos usuarios que han escrito al menos un informe alguna vez.

**Álgebra Relacional:**

$$\pi_{nombrePila, apellidos}(Voluntario \bowtie \rho_{idVoluntario \leftarrow idVoluntarioAutor}(\pi_{idVoluntarioAutor}(Informe)))$$


**SQL:**
```sql
SELECT DISTINCT V.nombrePila, V.apellidos
FROM Voluntario V
JOIN Informe I ON V.idVoluntario = I.idVoluntarioAutor;
```

#### f) Obtener el nombre y presupuesto de aquellos proyectos para los que se ha redactado al menos un informe en algún momento.

**Álgebra Relacional:**

$\pi_{nombre, presupuesto}(Proyecto \bowtie \pi_{idProyecto}(Informe))$


**SQL:**
```sql
SELECT DISTINCT P.nombre, P.presupuesto
FROM Proyecto P
JOIN Informe I ON P.idProyecto = I.idProyecto;
```

#### g) Obtener el nombre de aquellos proyectos con un presupuesto superior a 100.000 euros, y que cuentan con al menos un voluntario.

**Álgebra Relacional:**

$$\pi_{nombre}( (\sigma_{presupuesto > 100000}(Proyecto)) \bowtie (\pi_{idProyecto}(Participa)) )$$


**SQL:**
```sql
SELECT DISTINCT P.nombre
FROM Proyecto P
JOIN Participa Pa ON P.idProyecto = Pa.idProyecto
WHERE P.presupuesto > 100000;
```

#### h) Obtener las localidades de origen de los voluntarios de los proyectos iniciados en 2018 y con un presupuesto inferior a 50.000 euros.

**Álgebra Relacional:**

$$\pi_{localidad}( (\sigma_{añoInicio=2018 \text{ AND } presupuesto < 50000}(Proyecto)) \bowtie Participa \bowtie Voluntario )$$


**SQL:**
```sql
SELECT DISTINCT V.localidad
FROM Voluntario V
JOIN Participa Pa ON V.idVoluntario = Pa.idVoluntario
JOIN Proyecto Pr ON Pa.idProyecto = Pr.idProyecto
WHERE Pr.añoInicio = 2018 AND Pr.presupuesto < 50000;
```

#### i) Obtener el título de cada informe, además del nombre y apellidos de su autor y el nombre del proyecto al que corresponde.

**Álgebra Relacional:**

$$\pi_{titulo, nombrePila, apellidos, nombre}( (Informe \bowtie Proyecto) \bowtie \rho_{idVoluntario \leftarrow idVoluntarioAutor, nombrePila \leftarrow nombreAutor, apellidos \leftarrow apellidosAutor}(Voluntario) )$$

*(Nota: Se necesitan renombramientos más complejos si se quiere evitar colisión de nombres, pero para el Natural Join básico, el primer renombre es el clave)*

**SQL:**
```sql
SELECT I.titulo, V.nombrePila, V.apellidos, P.nombre
FROM Informe I
JOIN Voluntario V ON I.idVoluntarioAutor = V.idVoluntario
JOIN Proyecto P ON I.idProyecto = P.idProyecto;
```

#### j) Obtener el identificador, nombre de pila y apellidos de aquellos voluntarios que participan en TODOS los proyectos registrados en la BD.

**Álgebra Relacional:**

$$\pi_{idVoluntario, nombrePila, apellidos}( (\pi_{idVoluntario, idProyecto}(Participa) \div \pi_{idProyecto}(Proyecto)) \bowtie Voluntario )$$


**SQL:**
```sql
SELECT V.idVoluntario, V.nombrePila, V.apellidos
FROM Voluntario V
WHERE NOT EXISTS (
    SELECT idProyecto FROM Proyecto
    MINUS
    SELECT P.idProyecto FROM Participa P WHERE P.idVoluntario = V.idVoluntario
);
-- O, usando COUNT, que suele ser más portable:
SELECT V.idVoluntario, V.nombrePila, V.apellidos
FROM Voluntario V
JOIN Participa P ON V.idVoluntario = P.idVoluntario
GROUP BY V.idVoluntario, V.nombrePila, V.apellidos
HAVING COUNT(DISTINCT P.idProyecto) = (SELECT COUNT(idProyecto) FROM Proyecto);
```

#### k) Obtener el identificador y nombre de aquellos proyectos en los que participan TODOS los voluntarios registrados en la BD.

**Álgebra Relacional:**

$$\pi_{idProyecto, nombre}( (\pi_{idVoluntario, idProyecto}(Participa) \div \pi_{idVoluntario}(Voluntario)) \bowtie Proyecto )$$


**SQL:**
```sql
SELECT P.idProyecto, P.nombre
FROM Proyecto P
WHERE NOT EXISTS (
    SELECT idVoluntario FROM Voluntario
    MINUS
    SELECT Pa.idVoluntario FROM Participa Pa WHERE Pa.idProyecto = P.idProyecto
);
-- O, usando COUNT:
SELECT P.idProyecto, P.nombre
FROM Proyecto P
JOIN Participa Pa ON P.idProyecto = Pa.idProyecto
GROUP BY P.idProyecto, P.nombre
HAVING COUNT(DISTINCT Pa.idVoluntario) = (SELECT COUNT(idVoluntario) FROM Voluntario);
```

---

## Ejercicios Bloque 2

**Esquema:**
-   `Inv (id, nombre, pais)`
-   `Proy (pcode, nombre, fini, tema)`
-   `Partic (id, pcode, rol)`

#### a) Obtener el nombre de los investigadores que participaron en algún proyecto cuyo tema fuese 'ISS'.

**Álgebra Relacional:**

$$\pi_{nombre}( (\sigma_{tema='ISS'}(Proy)) \bowtie Partic \bowtie Inv )$$


**SQL:**
```sql
SELECT DISTINCT I.nombre
FROM Inv I
JOIN Partic Pa ON I.id = Pa.id
JOIN Proy Pr ON Pa.pcode = Pr.pcode
WHERE Pr.tema = 'ISS';
```

#### b) Obtener el nombre de los investigadores de Reino Unido ('UK') que participaron en algún proyecto cuyo nombre sea 'SUN'.

**Álgebra Relacional:**

$$\pi_{nombre}( (\sigma_{pais='UK'}(Inv)) \bowtie Partic \bowtie (\sigma_{nombre='SUN'}(Proy)) )$$


**SQL:**
```sql
SELECT DISTINCT I.nombre
FROM Inv I
JOIN Partic Pa ON I.id = Pa.id
JOIN Proy Pr ON Pa.pcode = Pr.pcode
WHERE I.pais = 'UK' AND Pr.nombre = 'SUN';
```

#### c) Obtener el nombre, código y tema de los proyectos en los que participaron todos los investigadores del país 'USA'.

**Álgebra Relacional:**

$$\pi_{pcode, nombre, tema}( (\pi_{id, pcode}(Partic) \div \pi_{id}(\sigma_{pais='USA'}(Inv))) \bowtie Proy)$$

*(Renombramos el `nombre` de `Proy` para la proyección final si fuera necesario para evitar ambigüedad, pero en este caso `pcode` y `tema` son suficientes para identificar el proyecto)*.

**SQL:**
```sql
SELECT Pr.pcode, Pr.nombre, Pr.tema
FROM Proy Pr
WHERE NOT EXISTS (
    SELECT I.id FROM Inv I WHERE I.pais = 'USA'
    MINUS
    SELECT Pa.id FROM Partic Pa WHERE Pa.pcode = Pr.pcode
);
-- O, usando COUNT:
SELECT Pr.pcode, Pr.nombre, Pr.tema
FROM Proy Pr
JOIN Partic Pa ON Pr.pcode = Pa.pcode
JOIN Inv I ON Pa.id = I.id AND I.pais = 'USA'
GROUP BY Pr.pcode, Pr.nombre, Pr.tema
HAVING COUNT(DISTINCT I.id) = (SELECT COUNT(id) FROM Inv WHERE pais = 'USA');
```
