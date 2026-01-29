# Dependencias en Procesadores Segmentados (MIPS)

Basado en el documento `T3 - 2.pdf` de la asignatura, se identifican tres tipos principales de dependencias que son cruciales para entender el funcionamiento de los procesadores segmentados (pipelined) como el MIPS que se simula en `Simula3MS`.

## 1. Dependencia Verdadera (RAW - Read After Write)

Es la dependencia de datos más común. Ocurre cuando una instrucción necesita leer un dato que todavía no ha sido escrito por una instrucción anterior. Esto genera un **riesgo de datos**.

- **Concepto:** Una instrucción `i` escribe en un registro y una instrucción posterior `j` lee de ese mismo registro. La instrucción `j` depende del resultado de `i`.

### Ejemplo para Simula3MS

```mips
# La instrucción 'add' intenta leer el registro $t0 en su etapa de decodificación (ID).
# Sin embargo, la instrucción 'addiu' que le precede solo tendrá el resultado listo
# al final de su etapa de ejecución (EX) y lo escribirá en el banco de registros
# al final de la etapa de escritura (WB).
#
# Sin mecanismos de adelantamiento (forwarding), el cauce debe detenerse (stall)
# hasta que el dato esté disponible, introduciendo burbujas.

.data
.text

main:
    addiu $t0, $zero, 10      # Instrucción i: Escribe el valor 10 en $t0
    add   $t1, $t0, $t0       # Instrucción j: Lee $t0 para calcular $t1 = 10 + 10
```

## 2. Dependencias de Nombre (Falsas Dependencias)

Estas dependencias no se deben a un flujo de datos, sino a que dos instrucciones usan el mismo nombre de registro para guardar resultados distintos. Se podrían eliminar renombrando los registros.

### a) Antidependencia (WAR - Write After Read)

Ocurre cuando una instrucción va a escribir en un registro que una instrucción *anterior* todavía tiene que leer.

- **Concepto:** La instrucción `i` lee un registro que una instrucción posterior `j` va a sobreescribir.

#### Ejemplo para Simula3MS

```mips
# En la pipeline MIPS de 5 etapas con ejecución en orden, esto NO suele generar un riesgo.
# La razón es que la lectura de operandos de la instrucción 'sub' (i) ocurre en una
# etapa temprana (ID, ciclo 1), mientras que la escritura del resultado de 'addiu' (j)
# ocurre en una etapa muy tardía (WB, ciclo 4). El orden se preserva de forma natural.
#
# Este conflicto es relevante en arquitecturas con ejecución fuera de orden.

.data
.text
main:
    sub $t2, $t1, $t3     # i: Lee el valor de $t1
    addiu $t1, $zero, 5   # j: Escribe un nuevo valor en $t1
```

### b) Dependencia de Salida (WAW - Write After Write)

Ocurre cuando dos instrucciones escriben en el mismo registro. Se debe garantizar que la escritura se realice en el orden del programa original.

- **Concepto:** La instrucción `i` y una instrucción posterior `j` escriben en el mismo registro de destino.

#### Ejemplo para Simula3MS

```mips
# Este conflicto puede generar un riesgo en procesadores con unidades funcionales
# de latencia variable (ej. una suma que tarda 2 ciclos y una multiplicación que tarda 5).
# La instrucción más rápida podría terminar después y escribir su resultado sobre el de
# la instrucción más lenta pero que empezó antes, violando el orden.
#
# En la pipeline simple de Simula3MS, donde las instrucciones tardan lo mismo, el orden
# de escritura en la etapa WB se mantiene y no hay riesgo.

.data
.text
main:
    add $t1, $t2, $t3     # i: Escribe en $t1
    sub $t1, $t4, $t5     # j: Vuelve a escribir en $t1. El resultado final debe ser el de SUB.
```

## 3. Dependencia de Control

Esta dependencia surge de las instrucciones de salto y bifurcación. El flujo de ejecución del programa depende del resultado de estas instrucciones, y el procesador no sabe qué instrucción cargar a continuación hasta que se evalúa la condición del salto. Esto genera **riesgos de control**.

- **Concepto:** La decisión de qué instrucción ejecutar a continuación depende del resultado de una instrucción de salto anterior.

### Ejemplo para Simula3MS

```mips
# Las instrucciones 'add' y 'sub' tienen una dependencia de control con 'beq'.
# El procesador, al buscar la siguiente instrucción después de 'beq', no sabe si
# cargar 'add' (si el salto no se toma) o la instrucción en la etiqueta 'saltar_aqui'
# (si el salto se toma).
#
# Esto se resuelve deteniendo el cauce (stall) hasta que 'beq' se resuelva en la
# etapa EX, o mediante técnicas más avanzadas como la predicción de saltos.

.data
.text
main:
    addiu $t0, $zero, 0   # Ponemos un 0 en $t0 para que la condición sea cierta

    beq $t0, $zero, saltar_aqui # Instrucción de salto condicional

    # --- Bloque dependiente del "no salto" ---
    add $s0, $s0, $s1   # Esta instrucción depende de que BEQ falle
    sub $s1, $s1, $s2   # Esta también
    j fin

saltar_aqui:
    # --- Bloque dependiente del "salto" ---
    addiu $s3, $s3, 1   # Esta es la que se ejecuta si el salto se toma

fin:
    # El programa continúa
```
