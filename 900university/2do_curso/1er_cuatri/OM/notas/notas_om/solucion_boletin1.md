# Solución Boletín 1 - Modelización en Programación Lineal

## Ejercicio 1: Centro de computación en la nube

### Enunciado
Un centro de computación en la nube quiere planificar el uso de sus servidores para ejecutar tres tipos de tareas relacionadas con inteligencia artificial:

*   **T1:** Entrenamiento de modelos de visión por computador.
*   **T2:** Entrenamiento de modelos de procesamiento de lenguaje natural.
*   **T3:** Ejecución de inferencias en tiempo real.

Cada tipo de tarea requiere una cierta cantidad de tiempo de CPU y de memoria GPU por hora de procesamiento, como se muestra en la tabla adjunta. El centro dispone de un máximo de 180 horas de CPU y de 90 GB de memoria GPU disponibles al día.
Se desea decidir cómo asignar cada tipo de tarea para maximizar el beneficio.

| Tipo de tarea | T1 | T2 | T3 |
| :--- | :--- | :--- | :--- |
| Horas de CPU por hora de tarea | 5 | 4 | 3 |
| Memoria GPU por hora de tarea (GB) | 2.5 | 3.0 | 1.5 |
| Beneficio por hora de tarea (euros) | 70 | 80 | 50 |

### Modelado

**Variables de decisión:**
*   $x_1$: Número de horas dedicadas a la tarea T1.
*   $x_2$: Número de horas dedicadas a la tarea T2.
*   $x_3$: Número de horas dedicadas a la tarea T3.

**Función Objetivo:**
Maximizar el beneficio total:
$$ \max Z = 70x_1 + 80x_2 + 50x_3 $$ 

**Restricciones:**
1.  Disponibilidad de horas de CPU:
    $$ 5x_1 + 4x_2 + 3x_3 \le 180 $$ 
2.  Disponibilidad de memoria GPU:
    $$ 2.5x_1 + 3.0x_2 + 1.5x_3 \le 90 $$ 
3.  No negatividad:
    $$ x_1, x_2, x_3 \ge 0 $$ 

---

## Ejercicio 2: Empresa de energías renovables

### Enunciado
Una empresa de energías renovables está planificando la producción diaria en tres plantas diferentes:

*   **P1:** Planta solar fotovoltaica.
*   **P2:** Parque eólico.
*   **P3:** Planta de biomasa.

La producción está limitada por distintos factores: horas de operación (480), consumo de agua (120), emisiones de CO2 (500) y capacidad máxima de la red eléctrica para recibir energía (150).
La empresa desea decidir cuántos MWh producir en cada planta para maximizar el beneficio total medido en euros.

| Recurso / Planta | P1 (Solar) | P2 (Eólica) | P3 (Biomasa) |
| :--- | :--- | :--- | :--- |
| Horas de operación por MWh | 4 | 3 | 5 |
| Consumo de agua ($m^3$) por MWh | 0 | 0 | 1.5 |
| Emisiones $CO_2$ (kg) por MWh | 0 | 0 | 8 |
| Beneficio por MWh | 45 | 40 | 55 |

### Modelado

**Variables de decisión:**
*   $x_1$: Cantidad de MWh producidos en la planta P1.
*   $x_2$: Cantidad de MWh producidos en la planta P2.
*   $x_3$: Cantidad de MWh producidos en la planta P3.

**Función Objetivo:**
Maximizar el beneficio:
$$ \max Z = 45x_1 + 40x_2 + 55x_3 $$ 

**Restricciones:**
1.  Límite de horas de operación:
    $$ 4x_1 + 3x_2 + 5x_3 \le 480 $$ 
2.  Límite de consumo de agua:
    $$ 1.5x_3 \le 120 $$ 
3.  Límite de emisiones de $CO_2$:
    $$ 8x_3 \le 500 $$ 
4.  Capacidad máxima de la red eléctrica:
    $$ x_1 + x_2 + x_3 \le 150 $$ 
5.  No negatividad:
    $$ x_1, x_2, x_3 \ge 0 $$ 

---

## Ejercicio 3: Transporte aéreo a aldeas

### Enunciado
Se dispone de tres tipos de aviones: A1, A2 y A3 para transportar sacos con alimentos desde un aeropuerto hasta cinco aldeas: V1, V2, V3, V4 y V5, afectadas por inundaciones. La cantidad de alimentos (en unidades adecuadas) que cada avión puede transportar a cada aldea en cada viaje, se da en la siguiente tabla. El número de viajes que puede hacer cada avión se da en la última columna y el número de vuelos que pueden realizarse sobre cada aldea diariamente en la última fila. Encontrar el número de viajes que deberá hacer cada avión a cada aldea de forma que se maximice la cantidad de alimento distribuido por día.

|                    | V1  | V2  | V3  | V4  | V5  | Capacidad Viajes/Día |
| :----------------- | :-- | :-- | :-- | :-- | :-- | :------------------- |
| **A1**             | 15  | 10  | 12  | 8   | 5   | 50                   |
| **A2**             | 5   | 5   | 10  | 4   | 8   | 90                   |
| **A3**             | 8   | 10  | 15  | 10  | 5   | 70                   |
| **Máx Vuelos/Día** | 100 | 80  | 70  | 30  | 20  |                      |

### Modelado

**Variables de decisión:**
*   $x_{ij}$: Número de viajes del avión de tipo $i$ a la aldea $j$, donde $i \in \{1, 2, 3\}$ (A1, A2, A3) y $j \in \{1, 2, 3, 4, 5\}$ (V1..V5).

**Función Objetivo:**
Maximizar la cantidad total de alimento distribuido:
$$ \max Z = 15x_{11} + 10x_{12} + 12x_{13} + 8x_{14} + 5x_{15} + 5x_{21} + 5x_{22} + 10x_{23} + 4x_{24} + 8x_{25} + 8x_{31} + 10x_{32} + 15x_{33} + 10x_{34} + 5x_{35} $$ 

**Restricciones:**
1.  Límite de viajes por tipo de avión (filas):
    *   A1: $\sum_{j=1}^{5} x_{1j} \le 50$
    *   A2: $\sum_{j=1}^{5} x_{2j} \le 90$
    *   A3: $\sum_{j=1}^{5} x_{3j} \le 70$

2.  Límite de vuelos sobre cada aldea (columnas):
    *   V1: $\sum_{i=1}^{3} x_{i1} \le 100$
    *   V2: $\sum_{i=1}^{3} x_{i2} \le 80$
    *   V3: $\sum_{i=1}^{3} x_{i3} \le 70$
    *   V4: $\sum_{i=1}^{3} x_{i4} \le 30$
    *   V5: $\sum_{i=1}^{3} x_{i5} \le 20$

3.  No negatividad e integridad (por ser número de viajes):
    $$ x_{ij} \ge 0, \quad x_{ij} \in \mathbb{Z} $$ 

---

## Ejercicio 4: Problema de la dieta

### Enunciado
En un hospital están interesados en diseñar el menú para un grupo de pacientes y para un día concreto basado en los siguientes ingredientes: pasta, pollo, patatas, espinacas y tarta de manzana. Partiendo de los requisitos nutricionales de este grupo de pacientes, un menú debe proporcionar como mínimo 64000 mg de proteínas, 15 mg de hierro y 50 mg de vitamina C. En la siguiente tabla se indican las cantidades de nutrientes y grasas (en mg) que corresponden a 100 gramos de cada uno de los ingredientes:

| Ingrediente | Proteínas | Hierro | Vitamina C | Grasa |
| :--- | :--- | :--- | :--- | :--- |
| **Pasta** | 4500 | 1.1 | 0 | 5000 |
| **Pollo** | 30300 | 1.8 | 0 | 5500 |
| **Patatas** | 5000 | 0.5 | 10 | 7900 |
| **Espinacas** | 3000 | 2.2 | 28 | 300 |
| **Tarta** | 3500 | 1.2 | 3 | 14300 |

Para que el menú sea lo más variado posible, no se deben sobrepasar los 200 gramos de pasta, 300 gramos de pollo, 200 gramos de patatas, 100 gramos de espinacas y 100 gramos de tarta de manzana. Formula un problema de programación matemática que permita determinar la composición del menú que satisfaga los requerimientos nutricionales y proporcione la mínima cantidad de grasa.

### Modelado

**Variables de decisión:**
*   $x_1$: Unidades de 100g de Pasta.
*   $x_2$: Unidades de 100g de Pollo.
*   $x_3$: Unidades de 100g de Patatas.
*   $x_4$: Unidades de 100g de Espinacas.
*   $x_5$: Unidades de 100g de Tarta.

**Función Objetivo:**
Minimizar la cantidad de grasa:
$$ \min Z = 5000x_1 + 5500x_2 + 7900x_3 + 300x_4 + 14300x_5 $$ 

**Restricciones:**
1.  Requerimiento de Proteínas:
    $$ 4500x_1 + 30300x_2 + 5000x_3 + 3000x_4 + 3500x_5 \ge 64000 $$ 
2.  Requerimiento de Hierro:
    $$ 1.1x_1 + 1.8x_2 + 0.5x_3 + 2.2x_4 + 1.2x_5 \ge 15 $$ 
3.  Requerimiento de Vitamina C:
    $$ 10x_3 + 28x_4 + 3x_5 \ge 50 $$ (Nota: Pasta y Pollo tienen 0).
4.  Límites de cantidad (variedad):
    *   Pasta $\le 200g$: $x_1 \le 2$
    *   Pollo $\le 300g$: $x_2 \le 3$
    *   Patatas $\le 200g$: $x_3 \le 2$
    *   Espinacas $\le 100g$: $x_4 \le 1$
    *   Tarta $\le 100g$: $x_5 \le 1$
5.  No negatividad:
    $$ x_i \ge 0 $$ 

---

## Ejercicio 5: Contratación de personal

### Enunciado
Una empresa de ingeniería ha decidido ampliar su plantilla contratando a matemáticos, físicos y/o informáticos. La siguiente tabla muestra las destrezas deseables para los nuevos empleados, la calificación esperada de matemáticos, físicos e informáticos en tales destrezas (atendiendo a la experiencia pasada de la empresa) y el coste mensual en euros de contratar a matemáticos, físicos e informáticos a tiempo completo (un trabajador a tiempo completo trabaja 160 horas mensuales). La empresa precisa, al menos, 960 horas-capacitadas semanales para cada destreza (las horas-capacitadas para una destreza se calculan sumando en el conjunto de empleados el producto de sus horas de trabajo por sus calificaciones en la destreza). Formula un problema de programación matemática que permita obtener el número de horas semanales que se precisan de cada profesional, de modo que el coste total sea mínimo.

*(Nota: Aunque el enunciado pide "número de horas semanales que se precisan de cada profesional", el contexto de "contratar ... a tiempo completo" y el coste mensual fijo sugiere que la variable natural es el número de trabajadores, o bien las horas totales. Dado que piden el coste mínimo, asumiremos que se pueden contratar fracciones de jornada o trabajadores completos. Modelaremos con número de trabajadores $x_i$ y asumiremos jornada completa de 160h/mes = 40h/semana).*

| | MAT | FIS | INF |
| :--- | :--- | :--- | :--- |
| Modelización | 8 | 9 | 8 |
| Análisis matemático | 9 | 8 | 7 |
| Programación | 8 | 6 | 9 |
| Manejo de herramientas informáticas | 7 | 7 | 9 |
| Interpretación de resultados | 9 | 9 | 9 |
| Comunicación | 7 | 7 | 8 |
| **Coste mensual** | 2.400 | 2.400 | 2.720 |

### Modelado

**Variables de decisión:**
*   $x_1$: Número de Matemáticos contratados.
*   $x_2$: Número de Físicos contratados.
*   $x_3$: Número de Informáticos contratados.

*Suposición: Cada trabajador aporta 40 horas semanales (160 mensuales / 4 semanas).*

**Función Objetivo:**
Minimizar el coste mensual:
$$ \min Z = 2400x_1 + 2400x_2 + 2720x_3 $$ 

**Restricciones:**
Requerimiento de 960 horas-capacitadas semanales para cada destreza.
Horas aportadas por trabajador = 40.
Calificación * Horas = Calificación * 40 * $x_i$.

1.  Modelización:
    $$ 40(8x_1 + 9x_2 + 8x_3) \ge 960 \implies 320x_1 + 360x_2 + 320x_3 \ge 960 $$ 
2.  Análisis matemático:
    $$ 40(9x_1 + 8x_2 + 7x_3) \ge 960 \implies 360x_1 + 320x_2 + 280x_3 \ge 960 $$ 
3.  Programación:
    $$ 40(8x_1 + 6x_2 + 9x_3) \ge 960 \implies 320x_1 + 240x_2 + 360x_3 \ge 960 $$ 
4.  Manejo de herramientas informáticas:
    $$ 40(7x_1 + 7x_2 + 9x_3) \ge 960 \implies 280x_1 + 280x_2 + 360x_3 \ge 960 $$ 
5.  Interpretación de resultados:
    $$ 40(9x_1 + 9x_2 + 9x_3) \ge 960 \implies 360x_1 + 360x_2 + 360x_3 \ge 960 $$ 
6.  Comunicación:
    $$ 40(7x_1 + 7x_2 + 8x_3) \ge 960 \implies 280x_1 + 280x_2 + 320x_3 \ge 960 $$ 
7.  No negatividad e integridad:
    $$ x_1, x_2, x_3 \ge 0, \quad x_i \in \mathbb{Z} $$ 

---

## Ejercicio 6: Planificación de proyectos (PERT/CPM)

### Enunciado
En la siguiente tabla se muestra el conjunto de actividades que componen un proyecto, sus duraciones en días y las relaciones de precedencia inmediata entre ellas. Se trata de plantear un problema de programación lineal cuya solución nos dé la duración mínima del proyecto.

| Actividad | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 | 9 | 10 |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| Duración | 3 | 2 | 6 | 2 | 3 | 3 | 5 | 1 | 1 | 2 |
| Precedentes | - | 1 | 1 | 3 | 1 | 2 | 3 | 3 | 6,8 | 9 |

### Modelado

**Variables de decisión:**
*   $t_i$: Tiempo de inicio de la actividad $i$ ($i=1..10$).
*   $T_{fin}$: Tiempo de finalización del proyecto.

**Función Objetivo:**
Minimizar el tiempo de finalización:
$$ \min T_{fin} $$ 

**Restricciones:**
Para cada actividad $j$ con precedente $i$, el inicio de $j$ debe ser posterior al fin de $i$ ($t_i + d_i$).
Duraciones $d = [3, 2, 6, 2, 3, 3, 5, 1, 1, 2]$.

1.  Actividades iniciales:
    $$ t_1 \ge 0 $$ 
2.  Precedencias:
    *   $t_2 \ge t_1 + 3$
    *   $t_3 \ge t_1 + 3$
    *   $t_4 \ge t_3 + 6$
    *   $t_5 \ge t_1 + 3$
    *   $t_6 \ge t_2 + 2$
    *   $t_7 \ge t_3 + 6$
    *   $t_8 \ge t_3 + 6$
    *   $t_9 \ge t_6 + 3$
    *   $t_9 \ge t_8 + 1$
    *   $t_{10} \ge t_9 + 1$
3.  Finalización del proyecto (debe ser posterior al fin de todas las actividades finales). Las actividades que no son precedentes de ninguna otra o que marcan el final son candidatas, pero $T_{fin}$ debe ser mayor o igual que el final de cualquier tarea.
    *   $T_{fin} \ge t_4 + 2$
    *   $T_{fin} \ge t_5 + 3$
    *   $T_{fin} \ge t_7 + 5$
    *   $T_{fin} \ge t_{10} + 2$

---

## Ejercicio 7: Formación de equipos

### Enunciado
Una empresa está interesada en lanzar un nuevo producto tecnológico. Para ello, debe formar un equipo de trabajo que cubra la lista de conocimientos que se indican en la tabla adjunta. Los empleados disponibles y sus conocimientos también se muestran en la tabla. Plantea un problema de programación lineal para diseñar un equipo adecuado con un número mínimo de miembros.

| Empleados | 1 | 2 | 3 | 4 | 5 | 6 |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| Tecnología inalámbrica | S | N | N | S | N | N |
| Lenguajes de programación | N | S | N | N | N | S |
| Sistemas operativos de móviles | N | N | S | N | S | S |
| Pantallas táctiles | N | S | S | N | N | S |
| Tratado de imágenes | S | S | S | N | N | N |
| Suministro de energía | N | S | S | N | N | S |

*(S = Sí, N = No)*

### Modelado

**Variables de decisión:**
*   $x_i$: Variable binaria que vale 1 si el empleado $i$ es seleccionado, 0 en caso contrario. $i \in \{1, \dots, 6\}$.

**Función Objetivo:**
Minimizar el número de miembros del equipo:
$$ \min Z = x_1 + x_2 + x_3 + x_4 + x_5 + x_6 $$ 

**Restricciones:**
Se debe cubrir cada conocimiento (al menos un empleado seleccionado debe tenerlo).

1.  Tecnología inalámbrica (Emp 1, 4):
    $$ x_1 + x_4 \ge 1 $$ 
2.  Lenguajes de programación (Emp 2, 6):
    $$ x_2 + x_6 \ge 1 $$ 
3.  Sistemas operativos de móviles (Emp 3, 5, 6):
    $$ x_3 + x_5 + x_6 \ge 1 $$ 
4.  Pantallas táctiles (Emp 2, 3, 6):
    $$ x_2 + x_3 + x_6 \ge 1 $$ 
5.  Tratado de imágenes (Emp 1, 2, 3):
    $$ x_1 + x_2 + x_3 \ge 1 $$ 
6.  Suministro de energía (Emp 2, 3, 6):
    $$ x_2 + x_3 + x_6 \ge 1 $$ 
7.  Variables binarias:
    $$ x_i \in \{0, 1\} $$ 

---

## Ejercicio 8: Producción de listones

### Enunciado
Una empresa produce listones de madera en cuatro medidas: pequeño, mediano, grande y extragrande. Estos listones pueden producirse en tres máquinas: A, B y C. La cantidad de metros que puede producir por hora cada máquina es:

| | A | B | C |
| :--- | :--- | :--- | :--- |
| **Pequeño** | 250 | 700 | 450 |
| **Mediano** | 275 | 500 | 800 |
| **Grande** | 180 | 350 | 500 |
| **Extragrande** | 100 | 50 | 350 |

Supongamos que cada máquina puede ser usada 70 horas semanales y que el coste operativo por hora de cada una es 70, 50 y 80 u.m., respectivamente. Si se necesitan 11000, 9000, 7000 y 5000 metros de cada tipo de listones por semana, plantear un modelo que nos planifique la producción minimizando costes.

### Modelado

**Variables de decisión:**
*   $x_{ij}$: Metros del listón tipo $i$ producidos en la máquina $j$.
    *   $i \in \{P, M, G, E\}$ (Pequeño, Mediano, Grande, Extragrande).
    *   $j \in \{A, B, C\}$ (Máquinas).

Alternativamente, podemos usar variables de **tiempo**:
*   $t_{ij}$: Horas que la máquina $j$ dedica a producir el listón tipo $i$.
    Esto simplifica la función de costes y las restricciones de capacidad.
    Cantidad producida = $t_{ij} \times \text{Tasa}_{ij}$.

*Usaremos las variables de tiempo $t_{ij}$ para facilitar el modelado.*

**Función Objetivo:**
Minimizar el coste operativo total:
$$ \min Z = 70\sum_{i} t_{iA} + 50\sum_{i} t_{iB} + 80\sum_{i} t_{iC} $$ 

**Restricciones:**
1.  Demanda de listones (Metros requeridos):
    *   Pequeño: $250t_{PA} + 700t_{PB} + 450t_{PC} \ge 11000$
    *   Mediano: $275t_{MA} + 500t_{MB} + 800t_{MC} \ge 9000$
    *   Grande: $180t_{GA} + 350t_{GB} + 500t_{GC} \ge 7000$
    *   Extragrande: $100t_{EA} + 50t_{EB} + 350t_{EC} \ge 5000$

2.  Capacidad de las máquinas (Horas semanales):
    *   Máquina A: $t_{PA} + t_{MA} + t_{GA} + t_{EA} \le 70$
    *   Máquina B: $t_{PB} + t_{MB} + t_{GB} + t_{EB} \le 70$
    *   Máquina C: $t_{PC} + t_{MC} + t_{GC} + t_{EC} \le 70$

3.  No negatividad:
    $$ t_{ij} \ge 0 $$ 

---

## Ejercicio 9: Turnos de enfermería/informáticos

### Enunciado
La administración de un complejo hospitalario ha estimado cuántos informáticos son necesarios para mantener los equipos del hospital en cada uno de los turnos de trabajo. Tales estimaciones se muestran en la tabla adjunta. El convenio establece que los informáticos comienzan a trabajar al principio de los turnos y trabajan ocho horas seguidas. La administración quiere determinar el número mínimo de informáticos a contratar para la atención correcta de los equipos del hospital. Formula un problema de programación lineal para resolver esta cuestión.

| Turno | 1 | 2 | 3 | 4 | 5 | 6 |
| :--- | :--- | :--- | :--- | :--- | :--- | :--- |
| Horas | 6-10 | 10-14 | 14-18 | 18-22 | 22-2 | 2-6 |
| Informáticos necesarios | 4 | 20 | 15 | 10 | 4 | 2 |

### Modelado

**Variables de decisión:**
*   $x_i$: Número de informáticos que comienzan su turno en el periodo $i$ ($i=1..6$).
    *   Como trabajan 8 horas, cubren el turno $i$ y el turno $i+1$ (o 1 si es el último).

**Función Objetivo:**
Minimizar el número total de informáticos contratados:
$$ \min Z = \sum_{i=1}^{6} x_i $$ 

**Restricciones:**
Cobertura de la demanda en cada turno (cada turno es cubierto por los que empiezan en él y los que empezaron en el turno anterior):

1.  Turno 1 (6-10): Cubierto por $x_1$ (6-14) y $x_6$ (02-10).
    $$ x_1 + x_6 \ge 4 $$ 
2.  Turno 2 (10-14): Cubierto por $x_2$ (10-18) y $x_1$ (6-14).
    $$ x_1 + x_2 \ge 20 $$ 
3.  Turno 3 (14-18):
    $$ x_2 + x_3 \ge 15 $$ 
4.  Turno 4 (18-22):
    $$ x_3 + x_4 \ge 10 $$ 
5.  Turno 5 (22-02):
    $$ x_4 + x_5 \ge 4 $$ 
6.  Turno 6 (02-06):
    $$ x_5 + x_6 \ge 2 $$ 
7.  No negatividad e integridad:
    $$ x_i \ge 0, \quad x_i \in \mathbb{Z} $$ 

---

## Ejercicio 10: Planificación financiera

### Enunciado
Un pequeño ahorrador tiene 1000 euros disponibles para invertirlos durante los próximos tres años. Al inicio de cada año puede invertir parte del dinero en depósitos a un año o a dos años. Los depósitos a un año pagan un interés del 4%, mientras que los depósitos a dos años pagan un 7% al final de los dos años. El objetivo es conseguir que al principio del cuarto año su capital sea lo mayor posible.

### Modelado

**Variables de decisión:**
*   $A_t$: Cantidad invertida en depósito a 1 año al inicio del año $t$ ($t=1, 2, 3$).
*   $B_t$: Cantidad invertida en depósito a 2 años al inicio del año $t$ ($t=1, 2$).
*   $R_t$: Dinero no invertido (líquido) guardado al inicio del año $t$ para el siguiente (asumiendo interés 0% para el dinero en mano).

**Función Objetivo:**
Maximizar el capital disponible al inicio del año 4:
$$ \max Z = R_3 + 1.04 A_3 + 1.07 B_2 $$ 

**Restricciones (Balance de flujos de caja):**
1.  Inicio Año 1: Disponemos de 1000 euros.
    $$ A_1 + B_1 + R_1 = 1000 $$ 
2.  Inicio Año 2: Disponible lo guardado ($R_1$) + retorno de inversión a 1 año hecha en año 1 ($1.04 A_1$).
    $$ A_2 + B_2 + R_2 = R_1 + 1.04 A_1 $$ 
3.  Inicio Año 3: Disponible lo guardado ($R_2$) + retorno de inversión a 1 año ($1.04 A_2$) + retorno de inversión a 2 años hecha en año 1 ($1.07 B_1$).
    $$ A_3 + R_3 = R_2 + 1.04 A_2 + 1.07 B_1 $$ 
    *(Nota: No se puede invertir en $B_3$ porque vencería en el año 5, fuera del horizonte).*

4.  No negatividad:
    $$ A_t, B_t, R_t \ge 0 $$ 

---

## Ejercicio 11: Compra de material informático

### Enunciado
Un departamento universitario ha dispuesto 12.000 euros de su presupuesto anual para la compra de material informático (ordenadores, impresoras y licencias de software). Cada uno de estos tres elementos tiene un coste unitario de 650, 420 y 720 euros, respectivamente. Por necesidades urgentes, deben comprarse al menos cinco licencias y dos impresoras. Debido a sus altos costes de mantenimiento, no pueden comprarse más de cinco impresoras. Según un acuerdo del departamento, la proporción de impresoras a ordenadores en cada compra ha de estar entre 1/5 y 1/2. Si las utilidades que se han asignado a ordenadores, impresoras y licencias son 2, 3 y 1, respectivamente, formula un problema de programación lineal para maximizar la utilidad de la compra atendiendo a todas las restricciones indicadas.

### Modelado

**Variables de decisión:**
*   $x_O$: Número de Ordenadores.
*   $x_I$: Número de Impresoras.
*   $x_L$: Número de Licencias.

**Función Objetivo:**
Maximizar la utilidad:
$$ \max Z = 2x_O + 3x_I + 1x_L $$ 

**Restricciones:**
1.  Presupuesto:
    $$ 650x_O + 420x_I + 720x_L \le 12000 $$ 
2.  Mínimo de licencias:
    $$ x_L \ge 5 $$ 
3.  Mínimo de impresoras:
    $$ x_I \ge 2 $$ 
4.  Máximo de impresoras:
    $$ x_I \le 5 $$ 
5.  Proporción Impresoras/Ordenadores ($1/5 \le x_I / x_O \le 1/2$):
    *   $x_I / x_O \ge 1/5 \implies 5x_I \ge x_O \implies x_O - 5x_I \le 0$
    *   $x_I / x_O \le 1/2 \implies 2x_I \le x_O \implies 2x_I - x_O \le 0$
6.  No negatividad e integridad:
    $$ x_O, x_I, x_L \ge 0, \quad x \in \mathbb{Z} $$ 

---

## Ejercicio 12: Diseño de encuesta

### Enunciado
Se pretende hacer una encuesta en España acerca del problema de la inmigración. A fin de que la misma sea significativa desde un punto de vista estadístico, se exige que ésta debe cumplir los siguientes requisitos:
1. Entrevistar al menos un total de 2.400 familias españolas.
2. De las familias entrevistadas, al menos 1100 deben cumplir que su cabeza de familia no supere los 30 años de edad.
3. Al menos 550 de las familias entrevistadas tendrán un cabeza de familia con edad comprendida entre los 31 y los 50 años.
4. El porcentaje de entrevistados que pertenecen a zonas con elevada tasa de inmigración no debe ser inferior a un 15% del total.
5. Finalmente, no más de un 30% de los entrevistados mayores de 50 años pertenecerán a zonas con alta tasa de inmigración.

Además, todas las encuestas deberán realizarse en persona. A continuación indicamos el coste en euros estimado de cada encuesta según la edad del encuestado y si procede o no de una zona con una alta tasa de inmigración:

| ZONA | Edad <31 | Edad 31-50 | Edad >50 |
| :--- | :--- | :--- | :--- |
| **Tasa inmig. alta** | 7.5 | 6.8 | 5.5 |
| **Tasa inmig. baja** | 6.9 | 7.25 | 6.1 |

Plantear un modelo de programación matemática para realizar el diseño de la encuesta de la forma más barata.

### Modelado

**Variables de decisión:**
$x_{ij}$: Número de encuestas a realizar en la zona $i$ del grupo de edad $j$.
*   $i \in \{A, B\}$ (A: Tasa Alta, B: Tasa Baja).
*   $j \in \{1, 2, 3\}$ (1: <31, 2: 31-50, 3: >50).

Variables explícitas:
*   $x_{A1}$: Alta, <31
*   $x_{B1}$: Baja, <31
*   $x_{A2}$: Alta, 31-50
*   $x_{B2}$: Baja, 31-50
*   $x_{A3}$: Alta, >50
*   $x_{B3}$: Baja, >50

**Función Objetivo:**
Minimizar el coste total:
$$ \min Z = 7.5x_{A1} + 6.9x_{B1} + 6.8x_{A2} + 7.25x_{B2} + 5.5x_{A3} + 6.1x_{B3} $$ 

**Restricciones:**
Sea $N$ el total de encuestas: $N = \sum x_{ij}$.

1.  Total de entrevistas:
    $$ x_{A1} + x_{B1} + x_{A2} + x_{B2} + x_{A3} + x_{B3} \ge 2400 $$ 
2.  Edad < 31:
    $$ x_{A1} + x_{B1} \ge 1100 $$ 
3.  Edad 31-50:
    $$ x_{A2} + x_{B2} \ge 550 $$ 
4.  Zona de alta inmigración $\ge$ 15% del total:
    $$ (x_{A1} + x_{A2} + x_{A3}) \ge 0.15 (x_{A1} + x_{B1} + x_{A2} + x_{B2} + x_{A3} + x_{B3}) $$ 
5.  Mayores de 50 en zona alta $\le$ 30% de los mayores de 50 totales:
    $$ x_{A3} \le 0.30 (x_{A3} + x_{B3}) $$ 
6.  No negatividad e integridad:
    $$ x_{ij} \ge 0, \quad x_{ij} \in \mathbb{Z} $$ 

---

## Ejercicio 13: Comisión universitaria

### Enunciado
Una universidad se encuentra en un proceso de formar una comisión. Hay 10 posibles candidatos: A, B, C, D, E, F, G, H, I y J. El reglamento obliga a que sean incluidos en dicha comisión al menos una mujer, un hombre, un estudiante, un administrativo y un profesor. Además, el número de mujeres debe ser igual que el de hombres y el número de profesores no debe de ser inferior al de administrativos. Teniendo en cuenta la siguiente información:

*   Mujeres: A, B, C, D, E
*   Hombres: F, G, H, I, J
*   Estudiantes: A, B, C, J
*   Administrativos: E, F
*   Profesores: D, G, H, I

Plantear un problema de programación lineal con el fin de que la comisión sea lo más reducida posible.

### Modelado

**Variables de decisión:**
*   $x_k$: Variable binaria (1 si es seleccionado, 0 si no) para el candidato $k \in \{A, B, C, D, E, F, G, H, I, J\}$.

**Función Objetivo:**
Minimizar el tamaño de la comisión:
$$ \min Z = \sum_{k \in \{A..J\}} x_k $$ 

**Restricciones:**
1.  Al menos una mujer:
    $$ x_A + x_B + x_C + x_D + x_E \ge 1 $$ 
2.  Al menos un hombre:
    $$ x_F + x_G + x_H + x_I + x_J \ge 1 $$ 
3.  Al menos un estudiante:
    $$ x_A + x_B + x_C + x_J \ge 1 $$ 
4.  Al menos un administrativo:
    $$ x_E + x_F \ge 1 $$ 
5.  Al menos un profesor:
    $$ x_D + x_G + x_H + x_I \ge 1 $$ 
6.  Igualdad de género (Mujeres = Hombres):
    $$ x_A + x_B + x_C + x_D + x_E = x_F + x_G + x_H + x_I + x_J $$ 
7.  Profesores $\ge$ Administrativos:
    $$ x_D + x_G + x_H + x_I \ge x_E + x_F $$ 
8.  Binarias:
    $$ x_k \in \{0, 1\} $$ 

---

## Ejercicio 14: Logística humanitaria

### Enunciado
Tras una serie de desastres naturales en distintas regiones, una ONG internacional debe planificar el envío diario de tres tipos de suministros esenciales desde su centro logístico central hacia varias zonas afectadas:

*   **S1:** Agua potable.
*   **S2:** Alimentos no perecederos.
*   **S3:** Medicamentos básicos.

Cada tipo de suministro requiere una cierta cantidad de camiones, horas de carga y debe cumplir con restricciones sanitarias y de capacidad de almacenamiento en destino. La ONG desea decidir cuántas toneladas de cada suministro enviar diariamente maximizar el impacto humanitario, medido en puntos de ayuda. Además el envío de medicamentos ha de superar las 10 toneladas.

| | S1 | S2 | S3 | Disponibilidad |
| :--- | :--- | :--- | :--- | :--- |
| Camiones | 0.8 | 0.6 | 0.4 | 30 |
| Horas de carga | 2 | 1.5 | 3 | 100 |
| Almacenamiento | 1.5 | 1.2 | 0.5 | 80 |
| Impacto | 50 | 40 | 70 | |

### Modelado

**Variables de decisión:**
*   $x_1$: Toneladas de Agua potable (S1).
*   $x_2$: Toneladas de Alimentos (S2).
*   $x_3$: Toneladas de Medicamentos (S3).

**Función Objetivo:**
Maximizar el impacto humanitario:
$$ \max Z = 50x_1 + 40x_2 + 70x_3 $$ 

**Restricciones:**
1.  Disponibilidad de camiones:
    $$ 0.8x_1 + 0.6x_2 + 0.4x_3 \le 30 $$ 
2.  Horas de carga:
    $$ 2x_1 + 1.5x_2 + 3x_3 \le 100 $$ 
3.  Almacenamiento en destino:
    $$ 1.5x_1 + 1.2x_2 + 0.5x_3 \le 80 $$ 
4.  Mínimo de medicamentos (Corrección de enunciado: El texto arriba dice "ha de superar las 10 toneladas", la tabla no pone límite, pero el texto manda):
    $$ x_3 > 10 $$ (En programación lineal estricta solemos usar $\ge$, y dado que es continua, $\ge 10$. Si es estrictamente mayor, numéricamente es $x_3 \ge 10 + \epsilon$, pero pondremos $\ge 10$ por convención estándar salvo que se especifique variable discreta).
    $$ x_3 \ge 10 $$ 
5.  No negatividad:
    $$ x_1, x_2, x_3 \ge 0 $$ 
