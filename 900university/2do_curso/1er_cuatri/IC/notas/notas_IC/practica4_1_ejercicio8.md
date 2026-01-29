# Boletín 4: Ejercicio 8 de `practica4_1_curso_2025-26_IC.pdf`

## Enunciado (Resumen)
- **Memoria Física:** 1 MiB, direccionable a nivel de byte.
- **Caché:** 256 bytes, con líneas de 16 bytes.
- **Secuencia de direcciones (hex):** `FF2A0`, `FF0A0`, `FC2A4`, `FF0A4`, `FF2A2`, `FF0A8`.
- **Objetivo:** Determinar el formato de las direcciones y el contenido final de la caché para distintos supuestos.

---
## a) La caché es de correspondencia directa

### Análisis del Formato de Dirección
Primero, calculamos los bits para cada campo de la dirección.

1.  **Bits de dirección física:** 1 MiB = 2^20 bytes -> **20 bits**.
2.  **Bits de desplazamiento (offset):** `log₂(Tamaño de Línea) = log₂(16) = 4` bits.
3.  **Número de líneas en caché:** `Tamaño Caché / Tamaño Línea = 256 / 16 = 16` líneas.
4.  **Bits de índice:** `log₂(Nº de Líneas) = log₂(16) = 4` bits.
5.  **Bits de etiqueta (tag)::** `Total - Índice - Desplazamiento = 20 - 4 - 4 = 12` bits.

El formato de la dirección de 20 bits queda así:

| TTTT TTTT TTTT | IIII | OOOO |
| :--------------: | :----: | :----: |
| **Etiqueta (12 bits)** | **Índice (4 bits)** | **Desplazamiento (4 bits)** |

### Análisis de la Secuencia de Accesos
Como se sospechaba, todas las direcciones de la secuencia apuntan al mismo índice `1010` (bin) = `0xA` (hex). Esto garantiza que todas competirán por la **Línea 10** de la caché.

### Trazado del Contenido de la Caché
La caché empieza vacía (todas las líneas inválidas).

| Nº | Dirección | Etiqueta | Índice | Despl. | Resultado | Contenido Final Línea 10 (Etiqueta) |
|---|---|---|---|---|---|---|
| 1 | `0xFF2A0` | `0xFF2` | `0xA` | `0x0` | **Fallo (obligatorio)** | `0xFF2` |
| 2 | `0xFF0A0` | `0xFF0` | `0xA` | `0x0` | **Fallo (conflicto)** | `0xFF0` (reemplaza a `0xFF2`) |
| 3 | `0xFC2A4` | `0xFC2` | `0xA` | `0x4` | **Fallo (conflicto)** | `0xFC2` (reemplaza a `0xFF0`) |
| 4 | `0xFF0A4` | `0xFF0` | `0xA` | `0x4` | **Fallo (conflicto)** | `0xFF0` (reemplaza a `0xFC2`) |
| 5 | `0xFF2A2` | `0xFF2` | `0xA` | `0x2` | **Fallo (conflicto)** | `0xFF2` (reemplaza a `0xFF0`) |
| 6 | `0xFF0A8` | `0xFF0` | `0xA` | `0x8` | **Fallo (conflicto)** | `0xFF0` (reemplaza a `0xFF2`) |

### Conclusión Final (Apartado a)

Al final de la secuencia:
- Se produce un **100% de fallos de caché**.
- La única línea de la caché que ha sido utilizada es la **línea 10 (índice 0xA)**.
- El bit de validez de la línea 10 está a `1`.
- La etiqueta almacenada en el directorio para la línea 10 es **`0xFF0`**, correspondiente al último acceso.
- Todas las demás líneas (0-9 y 11-15) permanecen inválidas.

Este es un ejemplo clásico de **conflicto de caché**, donde el rendimiento se degrada drásticamente porque múltiples bloques de memoria de uso frecuente mapean a la misma línea de caché.

---
## b) La caché es totalmente asociativa

En una caché totalmente asociativa, un bloque de memoria puede ir a cualquier línea libre. No existe el campo de índice. La dirección se divide únicamente en etiqueta y desplazamiento.

### Análisis del Formato de Dirección
1.  **Bits de desplazamiento:** `log₂(16) = 4` bits (sin cambios).
2.  **Bits de etiqueta:** `Total - Desplazamiento = 20 - 4 = 16` bits.

El formato de la dirección de 20 bits queda así:

| TTTT TTTT TTTT TTTT | OOOO |
| :---: | :---: |
| **Etiqueta (16 bits)** | **Desplazamiento (4 bits)** |

### Trazado del Contenido de la Caché
La caché tiene 16 líneas y la secuencia es solo de 6 accesos, por lo que nunca se llenará y no se producirán reemplazos.

| Nº | Dirección | Etiqueta | Despl. | Resultado | Contenido de la Caché (Línea: Etiqueta) |
|---|---|---|---|---|---|
| 1 | `0xFF2A0` | `0xFF2A` | `0x0` | **Fallo (obligatorio)** | Línea 0: `0xFF2A` |
| 2 | `0xFF0A0` | `0xFF0A` | `0x0` | **Fallo (obligatorio)** | Línea 0: `0xFF2A`, Línea 1: `0xFF0A` |
| 3 | `0xFC2A4` | `0xFC2A` | `0x4` | **Fallo (obligatorio)** | Línea 0: `0xFF2A`, Línea 1: `0xFF0A`, Línea 2: `0xFC2A` |
| 4 | `0xFF0A4` | `0xFF0A` | `0x4` | **ACIERTО** | (La etiqueta `0xFF0A` ya está en la línea 1) |
| 5 | `0xFF2A2` | `0xFF2A` | `0x2` | **ACIERTО** | (La etiqueta `0xFF2A` ya está en la línea 0) |
| 6 | `0xFF0A8` | `0xFF0A` | `0x8` | **ACIERTО** | (La etiqueta `0xFF0A` ya está en la línea 1) |

### Conclusión Final (Apartado b)
- **Fallos:** 3 (los 3 primeros, de tipo obligatorio).
- **Aciertos:** 3.
- La flexibilidad de la caché totalmente asociativa **evita por completo los fallos por conflicto** que plagaban la correspondencia directa en el apartado (a).
- **Contenido final:**
    - **Línea 0:** Etiqueta `0xFF2A`.
    - **Línea 1:** Etiqueta `0xFF0A`.
    - **Línea 2:** Etiqueta `0xFC2A`.
    - Las líneas 3 a 15 permanecen inválidas.

---
## c) La caché es asociativa por conjuntos de 4 vías

Esta es una solución híbrida. La caché se divide en conjuntos, y cada conjunto tiene 4 vías (líneas). Un bloque de memoria mapea a un conjunto específico, pero puede ocupar cualquier vía libre dentro de ese conjunto.

### Análisis del Formato de Dirección
1.  **Número de conjuntos:** `Nº Líneas / Vías por Conjunto = 16 / 4 = 4` conjuntos.
2.  **Bits de índice (para el conjunto):** `log₂(Nº Conjuntos) = log₂(4) = 2` bits.
3.  **Bits de desplazamiento:** 4 bits (sin cambios).
4.  **Bits de etiqueta:** `Total - Índice - Desplazamiento = 20 - 2 - 4 = 14` bits.

El formato de la dirección de 20 bits es:

| TTTT TTTT TTTT TT | II | OOOO |
| :---: | :---: | :---: |
| **Etiqueta (14 bits)** | **Índice (2 bits)** | **Desplazamiento (4 bits)** |

### Trazado del Contenido de la Caché
Se observa que todas las direcciones de la secuencia tienen los bits de índice `10`, por lo que todas mapean al **Conjunto 2**. Este conjunto tiene 4 vías, que son suficientes para alojar las 3 etiquetas únicas de la secuencia sin reemplazos.

| Nº | Dirección | Etiqueta (14b) | Índice | Resultado | Estado del Conjunto 2 (Vía: Etiqueta) |
|---|---|---|---|---|---|
| 1 | `0xFF2A0` | `0x3FD2` | 2 | **Fallo (obligatorio)** | Vía 0: `0x3FD2` |
| 2 | `0xFF0A0` | `0x3FC2` | 2 | **Fallo (obligatorio)** | Vía 0: `0x3FD2`, Vía 1: `0x3FC2` |
| 3 | `0xFC2A4` | `0x3F0A` | 2 | **Fallo (obligatorio)** | Vía 0: `0x3FD2`, Vía 1: `0x3FC2`, Vía 2: `0x3F0A`|
| 4 | `0xFF0A4` | `0x3FC2` | 2 | **ACIERTО** | (La etiqueta `0x3FC2` ya está en la Vía 1) |
| 5 | `0xFF2A2` | `0x3FD2` | 2 | **ACIERTО** | (La etiqueta `0x3FD2` ya está en la Vía 0) |
| 6 | `0xFF0A8` | `0x3FC2` | 2 | **ACIERTО** | (La etiqueta `0x3FC2` ya está en la Vía 1) |

### Conclusión Final (Apartado c)
- **Fallos:** 3 (obligatorios).
- **Aciertos:** 3.
- El grado de asociatividad (4 vías) fue suficiente para absorber las peticiones al mismo conjunto sin causar fallos por conflicto, dando un resultado idéntico al de la caché totalmente asociativa para esta secuencia.
- **Contenido final:**
    - **Conjunto 2, Vía 0:** Etiqueta `0x3FD2`.
    - **Conjunto 2, Vía 1:** Etiqueta `0x3FC2`.
    - **Conjunto 2, Vía 2:** Etiqueta `0x3F0A`.
    - **Conjunto 2, Vía 3:** Inválida.
    - El resto de conjuntos (0, 1, 3) permanecen completamente inválidos.
