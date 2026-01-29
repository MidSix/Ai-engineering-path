# Paginación Multinivel

#tags: memoria_virtual, paginacion, arquitectura

La paginación es la técnica que permite traducir direcciones virtuales en direcciones físicas. El esquema más simple es la **Paginación de 1 Nivel**, pero presenta un problema grave en sistemas con espacios de direcciones virtuales grandes. La paginación multinivel resuelve este problema.

---
## El Problema: El Tamaño de la Tabla de Páginas de 1 Nivel

En una **Paginación de 1 Nivel** (también llamada **traducción directa**), se usa una única tabla de páginas por proceso. El Número de Página Virtual (NPV) sirve como índice directo a esta tabla para encontrar el marco físico.

- **Dirección Virtual:** `[Número de Página Virtual | Desplazamiento]`

El problema es que esta tabla puede ser **enorme**. Por ejemplo, en un sistema de 32 bits (4 GiB de espacio virtual) con páginas de 4 KiB, un proceso podría tener hasta `2^20` páginas virtuales (`4 GiB / 4 KiB`). Si cada entrada de la tabla de páginas (PTE) ocupa 4 bytes, la tabla de páginas completa ocuparía `2^20 * 4 B = 4 MiB` de memoria RAM.

Tener que almacenar una tabla de 4 MiB contiguos en RAM para cada proceso es muy ineficiente y, a menudo, inviable.

---
## La Solución: Paginación de Múltiples Niveles

La paginación multinivel resuelve este problema tratando a la propia tabla de páginas como un objeto más que puede ser paginado. En lugar de una única tabla monolítica, se crea una jerarquía de tablas (una estructura de árbol).

### Ejemplo: Paginación de 2 Niveles

Es el caso más común para ilustrar el concepto. La tabla de páginas se divide en dos niveles:
1.  **Directorio de Páginas (Tabla de Nivel 1):** Una tabla de primer nivel.
2.  **Tabla de Páginas (Tabla de Nivel 2):** Múltiples tablas de segundo nivel.

La dirección virtual se divide ahora en tres partes:

- **Dirección Virtual:** `[Índice Nivel 1 (P1) | Índice Nivel 2 (P2) | Desplazamiento]`

El proceso de traducción es el siguiente:
1.  Se usa el campo **P1** como índice para entrar en el **Directorio de Páginas (Nivel 1)**.
2.  La entrada obtenida **no es el marco físico**, sino la **dirección base de la tabla de Nivel 2** que se necesita.
3.  Se usa el campo **P2** como índice para entrar en esa **tabla de Nivel 2** específica.
4.  La entrada obtenida de la tabla de Nivel 2 **sí contiene el Número de Marco Físico (NMF)**.
5.  Se combina el NMF con el Desplazamiento para obtener la dirección física final.

**Ventajas:**
- **Ahorro de espacio:** No es necesario que todas las tablas de Nivel 2 estén en memoria. Solo se cargan el directorio de Nivel 1 y las tablas de Nivel 2 que se estén usando activamente.

**Desventajas:**
- **Mayor latencia:** Una traducción de 2 niveles requiere **dos accesos a memoria** (uno para la tabla L1, otro para la L2) en el peor de los casos, en lugar de uno solo. Esto hace que el uso de la **TLB (Translation Lookaside Buffer)** sea aún más crucial para cachear las traducciones y evitar estos accesos múltiples.

### Generalización a `n` Niveles

Este concepto se extiende a 3, 4 o más niveles, especialmente en sistemas de 64 bits, donde los espacios de direcciones virtuales son inmensos y una tabla de un solo nivel sería impensablemente grande. Para cada traducción, se requerirán `n` accesos a memoria en el peor de los casos.
