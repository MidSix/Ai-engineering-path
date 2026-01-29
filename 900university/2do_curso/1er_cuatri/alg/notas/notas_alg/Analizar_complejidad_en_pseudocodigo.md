
Esta nota detalla el procedimiento para analizar la complejidad temporal de un algoritmo expresado en pseudocódigo, línea a línea, basándose en las reglas del **Modelo de Computación** y el cálculo asintótico.

## 1. Operaciones Elementales (Regla Básica)

Cualquier operación simple que toma un tiempo constante, acotado superiormente por una constante, tiene coste $O(1)$. 

*   **Asignaciones:** `x := 5`
*   **Operaciones aritméticas:** `x + y`, `x * y` (con tipos básicos)
*   **Comparaciones lógicas:** `a < b`, `x = y`
*   **Acceso a memoria/array:** `v[i]`
*   **Retorno de función:** `RETORNAR x`

> **Regla:** Si una línea contiene solo operaciones elementales, su complejidad es $O(1)$.

### Aclaración sobre Cierres y Cabeceras
Es una duda común: **¿Cuentan las líneas de cierre (`FIN PARA`, `FIN SI`)?**
*   **No**, generalmente se considera que tienen coste $0$ o que su coste de gestión (saltos de memoria) está implícito en la **cabecera**.
*   **Cabeceras de Bucle (`PARA`, `MIENTRAS`):** Llevan el peso del control. En un bucle de $n$ iteraciones, la cabecera se ejecuta teóricamente **$n+1$ veces** (las $n$ veces que entra + la última vez que comprueba la condición, falla y sale).
*   **Cierres (`FIN ...`):** Son puramente sintácticos para delimitar el bloque.

## 2. Estructuras de Control

### Secuencia (Bloques de código)
Para una secuencia de instrucciones $S_1; S_2; ...; S_k$, la complejidad total es la suma de las complejidades individuales. En notación asintótica, domina el término de mayor orden (regla de la suma).

$$T(n) = \max(O(f_{S1}(n)), O(f_{S2}(n)), ...)$$

**Ejemplo:**
```text
x := 0          // O(1)
PARA i := 1...n hacer// O(n) 
    ...
FIN PARA
// Total: O(1) + O(n) -> O(max(1,n)) -> O(n) porque n domina a 1.
```

### Condicionales (SI / ENTONCES / SINO)
El coste de una estructura `SI <condición> ENTONCES <rama_A> SINO <rama_B>` es el coste de evaluar la condición más el coste de la rama que tome más tiempo (**peor caso**).

$$T(n) = T_{condición} + \max(T_{rama\_A}, T_{rama\_B})$$

**Ejemplo:**
```text
SI x > 0 ENTONCES           // Condición: O(1)
    x <- x + 1              // Rama A: O(1)
SINO
    OrdenacionInsercion(v)  // Rama B: O(n^2)
FIN SI
// Total: O(1) + max(O(1), O(n^2)) = O(n^2)


//Los condicionales a diferencia de los bucles no dependen del cuerpo para
//establecer su complejidad, no tienen nada de relacion, de hecho. Solo miras
//la operacion que se haga en el condicional que usualmente son comparaciones y 
//demas, por lo tanto la complejidad temporal usual de un condicional es constante
//O(1). en el caso de condicionales sin condicion, como el SINO(python el "else")
//el coste es despreciable, con lo cual no se escriba nada en esa linea. Y en los //cierres ya sea de condicionales, bucles, funciones tampoco se escribe nada ya que
//tienen coste despreciable.
```

### Bucles (PARA / MIENTRAS - for loops y while loops)
El tiempo de ejecución es la suma de los costes de cada iteración.

$$T(n) = \sum_{i=1}^{k} T_{cuerpo}(i)$$

Si el coste del cuerpo es el mismo en cada iteración ($C$) y el número de iteraciones es $k(n)$:
>fundamental esta parte de aqui. Esto es fundamental para calcular la complejidad temporal de todo bucle.
$$T(n) = k(n) \times C$$

#### Bucle Simple
```text
PARA i <- 1 HASTA n HACER   // Se ejecuta n veces
    x <- x + 1              // Cuerpo: O(1)
FIN PARA
// Total: n * O(1) = O(1 * n) = O(n)
```

#### Bucles Anidados (Independientes)
Si el bucle interno no depende del índice del externo:
```text
PARA i <- 1 HASTA n HACER       // n iteraciones
    PARA j <- 1 HASTA n HACER   // n iteraciones
        x <- x + 1              // O(1)
    FIN PARA
FIN PARA
// Total: n * n * O(1) = O(n * n * 1) = O(n^2)
```

#### Bucles Anidados (Dependientes)
Si el bucle interno depende del externo (ej. `j` va hasta `i`):
```text
PARA i <- 1 HASTA n HACER       // n iteraciones
    PARA j <- 1 HASTA i HACER   // i iteraciones (1, 2, ..., n)
        x <- x + 1              // O(1)
    FIN PARA
FIN PARA
```
Se calcula la suma de la serie aritmética:
>Bueno esa formula la va a aprender su madre, basicamente tratalos como si fueran independientes y ya esta, si llegase a estar mal en algun caso pues te restan algo de nota y ya esta, tampoco es para tanto.
$$\sum_{i=1}^{n} i = \frac{n(n+1)}{2} = \frac{n^2 + n}{2} = O(n^2)$$
## Ejemplo jodido:
```text
para i := 1 hasta n hacer                 // bucle exterior || respuesta :O(n^4)
    insertion_sort(...)                   // O(n^2)

    para j := 1 hasta n hacer             // bucle interior || respuesta O(n^3)
        operación constante               // O(1)
        insertion_sort(...)               // O(n^2)
    fin para
fin para
```

* *Razonamiento*: Para calcular la complejidad temporal de todo bucle(para-mientras/for-while) necesitamos la complejidad de su cuerpo. Con lo cual entramos dentro del bucle i, vemos que tiene insertion_sort O(n^2) y otro bucle j. Necesitamos calcular la complejidad de ese bucle j entonces nos metemos dentro de j. Dentro de j tenemos una operacion contante O(1) y otra vez insertion_sort(este ejemplo no tiene ningun sentido, solo es para aprender las reglas xd). Pues para calcular la complejidad del bucle j tenemos que sumar la complejidad de cada una de sus lineas, O(1) + O(n^2), la suma de complejidades esta definida de manera que se busca la complejidad mas alta, la dominante:  O(1) + O(n^2) = O(max(1,n^2)) = O(n^2). Ya tenemos la complejidad temporal del cuerpo del bucle j. Ahora todo bucle tiene un numero determinado de iteraciones, las llamaremos n, entonces la complejidad del cuerpo se repite n veces. Pues multiplicas la complejidad del cuerpo por n -> n * O(n^2), la multiplicacion de complejidades se define asi:  n * O(n^2) = O(n * n^2) = O(n^3). Ahora volvamos al bucle i. tiene O(n^2) en insertion_sort y O(n^3) en el bucle j. Pues la complejidad de su cuerpo:               O(n^2) + O(n^3) = O(max(n^2, n^3)) = O(n^3). Ahora por ultimo, lo que queremos. Calcular la complejidad del bucle i. Ya sabemos que la complejidad de su cuerpo es O(n^3), ahora hacemos n * O(n^3) = O(n^3) = O(n^4). --> *Bucle i (bucle exterior) -> O(n^4)*.

## 3. Llamadas a Funciones y Recursividad

### Llamadas no recursivas
Simplemente se sustituye la llamada por su coste ya analizado. Si `Ordenar(v)` es $O(n^2)$, la línea donde se llama cuenta como $O(n^2)$.

### Llamadas Recursivas
Para analizar funciones recursivas, se debe plantear una **Ecuación de Recurrencia** $T(n)$.

1.  **Identificar el Caso Base:** Cuando la recursión se detiene (generalmente $n$ pequeño). Coste $C$ (cte).
2.  **Identificar el Caso Recursivo:** Coste de dividir el problema + coste de las llamadas recursivas + coste de combinar resultados.

**Formato general(Teorema de divide y venceras):**
$$T(n) = l \cdot T(n/b) + f(n)$$
*   $l$: Número de llamadas recursivas.
*   $b$: Factor de reducción del tamaño del problema.
*   $f(n)$: Coste del resto de operaciones fuera de la llamada (dividir/combinar).

#### Ejemplo: Búsqueda Binaria
```text
FUNCION BusquedaBinaria(v, i, j, x)
    SI i > j ENTONCES                       // O(1)
	    RETORNAR -1                         // O(1)
    FIN SI                                 
	    medio <- (i + j) DIV 2              // O(1)
	SI v[medio] = x      ENTONCES           // O(1)
		RETORNAR medio                      // O(1)
    SINO SI x < v[medio] ENTONCES           // O(1)
        RETORNAR BusquedaBinaria(..., medio-1, ...) //1 llamada,T(n/2) || O(logn)
    SINO                                  
        RETORNAR BusquedaBinaria(..., medio+1, ...) //1 llamada,T(n/2) || O(logn)
    Fin SI                                 
FIN FUNCION                                
```
**Recurrencia:**
*   $T(n) = 1\cdot T(n/2) + O(1)$
*   Por Teorema Maestro(Teorema de divide y venceras) ($l=1, b=2, k=0$): 
*  llegamos a que L = b^k, 1 = 2^0 -> del teorema concluimos que tiene O(n^k log(n)).  
>$T(n) = O(1*\log n) = O(\log n)$.

#### Ejemplo: Mergesort
*   Divide en 2 mitades ($b=2$).
*   Hace 2 llamadas recursivas ($l=2$).
*   Combina las soluciones en tiempo lineal ($f(n) = cn^1 \rightarrow k = 1$).
*   Recurrencia: $T(n) = 2T(n/2) + cn$.
*   Por Teorema Maestro ($l=2, b=2, k=1 \rightarrow l=b^k$): $T(n) = O(n \log n)$.

**Recursos Relacionados:**
- [[algoritmos_del_tema1_analisis_de_algoritmos]]
- [[20260117_Teorema_Maestro_Recurrencias]]
