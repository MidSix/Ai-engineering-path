# Práctica 4.2: Ejercicio 3 - Análisis Detallado

Este ejercicio requiere trazar el comportamiento de una caché con políticas de escritura complejas (`write-back` y `write-allocate`) mientras se ejecuta un bucle en ensamblador.

### Aclaración Clave: ¿Por qué `lw` y `sw` acceden a memoria si usan registros?

Esta es la duda fundamental para entender el ejercicio. Aunque una instrucción como `lw $t0, 0($a0)` usa registros (`$t0`, `$a0`) que están dentro de la CPU, su propósito es comunicar la CPU con la jerarquía de memoria.

- **`lw` significa "Load Word" (Cargar Palabra):** Su misión es traer un dato **desde la memoria** y guardarlo en un registro.
- **`sw` significa "Store Word" (Almacenar Palabra):** Su misión es tomar un dato de un registro y guardarlo **en la memoria**.

En la instrucción `lw $t0, 0($a0)`:
- `$t0` es el registro **destino** donde se guardará el dato traído de memoria.
- `0($a0)` **no es un valor, es un cálculo de dirección**. Significa: "coge el valor que hay en el registro `$a0`, súmale `0`, y el resultado es la **dirección de memoria** a la que debes ir". El registro `$a0` se usa como un **puntero**.

A diferencia de una instrucción como `add $t2, $t0, $t1`, que opera exclusivamente con datos que ya están en registros, las instrucciones `lw` y `sw` son las únicas que cruzan la frontera entre la CPU y el sistema de memoria. Por tanto, **cada `lw` y `sw` del código representa un acceso a memoria** que debe ser analizado por la caché.

---

### Paso 1: Generar la Secuencia de Accesos a Memoria

El programa ejecuta un bucle 4 veces. Analizando el código, la secuencia de accesos a memoria (que primero consultan la caché) es la siguiente:

- **Iteración 1:** Leer `0x1004`, Leer `0x2004`, Escribir `0x2004`
- **Iteración 2:** Leer `0x1008`, Leer `0x2008`, Escribir `0x2008`
- **Iteración 3:** Leer `0x100C`, Leer `0x200C`, Escribir `0x200C`
- **Iteración 4:** Leer `0x1010`, Leer `0x2010`, Escribir `0x2010`
- **Post-bucle:** Leer `0x3004`

### Paso 2: Explicación de las Políticas de Caché

- **Ubicar en Escritura (Write-Allocate):** En un fallo de escritura, primero se trae el bloque de memoria a la caché y luego se escribe sobre la copia en caché. Un fallo de escritura se comporta de forma muy similar a un fallo de lectura.

- **Post-escritura (Write-Back):** Las escrituras solo modifican la copia en caché. Se usa un **Bit de Modificación (BM)** o "dirty bit" para rastrear los bloques modificados. Un bloque solo se escribe de vuelta a la memoria RAM cuando es **reemplazado** y está "sucio" (BM=1).

### Paso 3: Análisis para Correspondencia Directa

#### Formato de la Dirección
- **Memoria Principal:** 2 MiB -> **21 bits de dirección**.
- **Líneas en Caché:** 1 KiB / 16 B = 64 líneas.
- **Bits de Índice:** `log₂(64) = 6` bits.
- **Bits de Desplazamiento:** `log₂(16) = 4` bits.
- **Bits de Etiqueta:** `21 - 6 - 4 = 11` bits.
- **Formato:** `[Etiqueta (11) | Índice (6) | Desplazamiento (4)]`

#### Descomposición de Direcciones Clave
- `0x...004` -> `... 000001 0100` -> Índice `1`
- `0x...008` -> `... 000010 1000` -> Índice `2`
- `0x...00C` -> `... 000011 1100` -> Índice `3`
- `0x...010` -> `... 000100 0000` -> Índice `4`

#### Trazado Completo de Accesos
>En el comentario de los cache miss (F) se especifica que no son bloques sucios, es decir, que su bit de modificacion es 0, porque de ser sucios, es decir, que su bit de modificacion sea 1, que el bloque de memoria guardado en esa linea cache haya sido modificado, implicaria que antes de hacer el reemplazo de RAM a Cache habria que hacer un reemplazo Cache - Ram de ese bloque para mantener consistencia entre la cache y la Ram SIN EMBARGO, ese comentario no tiene nada que ver con si es un fallo o no, es simplemente un comentario necesario para describir las consecuencias de que se haya producido un fallo pero no es en ningun caso una condicion para que se haya producido el fallo en si mismo.

| Nº  | Tipo | Dirección | Etiqueta | Índice | A/F   | Reempl. | MP Write | Comentario                                                                                                             |
| :-- | :--- | :-------- | :------- | :----- | :---- | :------ | :------- | :--------------------------------------------------------------------------------------------------------------------- |
| 1   | R    | `0x1004`  | `0x080`  | 1      | **F** | No      | No       | Fallo obligatorio. Se carga el bloque.                                                                                 |
| 2   | R    | `0x2004`  | `0x100`  | 1      | **F** | Sí      | No       | Conflicto. La línea 1 tenía Et: `0x080` (no sucio).                                                                    |
| 3   | W    | `0x2004`  | `0x100`  | 1      | **A** | No      | No       | Acierto. La línea 1 (Et: `0x100`) se marca como sucia (BM=1).                                                          |
| 4   | R    | `0x1008`  | `0x080`  | 2      | **F** | No      | No       | Fallo obligatorio.                                                                                                     |
| 5   | R    | `0x2008`  | `0x100`  | 2      | **F** | Sí      | No       | Conflicto. La línea 2 tenía Et: `0x080` (no sucio).                                                                    |
| 6   | W    | `0x2008`  | `0x100`  | 2      | **A** | No      | No       | Acierto. La línea 2 (Et: `0x100`) se marca como sucia (BM=1).                                                          |
| 7   | R    | `0x100C`  | `0x080`  | 3      | **F** | No      | No       | Fallo obligatorio.                                                                                                     |
| 8   | R    | `0x200C`  | `0x100`  | 3      | **F** | Sí      | No       | Conflicto. La línea 3 tenía Et: `0x080` (no sucio).                                                                    |
| 9   | W    | `0x200C`  | `0x100`  | 3      | **A** | No      | No       | Acierto. La línea 3 (Et: `0x100`) se marca como sucia (BM=1).                                                          |
| 10  | R    | `0x1010`  | `0x080`  | 4      | **F** | No      | No       | Fallo obligatorio.                                                                                                     |
| 11  | R    | `0x2010`  | `0x100`  | 4      | **F** | Sí      | No       | Conflicto. La línea 4 tenía Et: `0x080` (no sucio).                                                                    |
| 12  | W    | `0x2010`  | `0x100`  | 4      | **A** | No      | No       | Acierto. La línea 4 (Et: `0x100`) se marca como sucia (BM=1).                                                          |
| 13  | R    | `0x3004`  | `0x180`  | 1      | **F** | Sí      | **Sí**   | Conflicto en línea 1. El bloque con Et: `0x100` estaba sucio (BM=1), forzando una escritura a RAM antes del reemplazo. |

### Estado Final del Directorio de Caché (Solo líneas afectadas)

| Índice | BV (Válido) | BM (Modificado) | Etiqueta |
| :----- | :---------- | :-------------- | :------- |
| 1      | 1           | 0               | `0x180`  |
| 2      | 1           | 1               | `0x100`  |
| 3      | 1           | 1               | `0x100`  |
| 4      | 1           | 1               | `0x100`  |

*(Todas las demás líneas permanecen con BV=0)*
