# 20260111163000 Dependencias Funcionales: Semántica vs. Extensión en Diseño Lógico Ascendente

**Etiquetas:** #bases_de_datos #diseño_lógico #dependencias_funcionales #semántica #diseño_ascendente

## Concepto Clave: DFs como Propiedades Semánticas

Las dependencias funcionales (DFs) son fundamentales en el diseño lógico de bases de datos, especialmente en el enfoque ascendente (bottom-up), donde se utilizan para sintetizar esquemas de relación. Es crucial entender que:

> "Las dependencias funcionales son propiedades de la semántica o del significado de los atributos, no de la extensión (o estado) de una relación en un momento dado."
> *(J. R. Paramá, Diseño Lógico)*

### Explicación

Esto significa que una DF (`X -> Y`) representa una **regla de negocio inherente** o una **restricción lógica** basada en el significado real de los datos en el mundo. No se derivan de la mera observación de los valores que existen en una tabla en un instante específico.

*   **Propiedad Semántica:** La existencia de una DF se establece porque, por la propia naturaleza o definición de los atributos, un conjunto de atributos (`X`) *siempre* determinará unívocamente a otro conjunto (`Y`), bajo cualquier estado válido de la base de datos. Es una propiedad de la realidad que la BD modela.
*   **Independiente de la Extensión:** No se debe inferir una DF basándose únicamente en los datos actuales ("la foto" de la tabla). Si solo se observa el "estado" actual, se podrían establecer dependencias que son meras coincidencias temporales y no reglas universales del negocio. Estas "dependencias accidentales" desaparecerían al cambiar los datos.

### Implicación en el Diseño Lógico Ascendente

En el diseño lógico ascendente (donde se parte de un conjunto de atributos y DFs para construir las tablas), la correcta identificación de las DFs basadas en la semántica es vital para:

1.  **Evitar un diseño incorrecto:** No crear DFs erróneas (por casualidad en los datos) que lleven a descomposiciones innecesarias o, peor, a no identificar DFs reales que resulten en anomalías.
2.  **Garantizar la Robustez:** Asegurar que el esquema de la base de datos sea válido y consistente incluso cuando los datos cambien drásticamente a lo largo del tiempo. Las DFs actúan como los cimientos lógicos que estructuran el modelo relacional.
3.  **Reflejar la Realidad:** Que el diseño de la base de datos sea un reflejo fiel de las reglas y relaciones del mundo real que se intentan modelar.

### Ejemplo

Considera una tabla con `(Ciudad, CódigoPostal)`:
*   La DF `CódigoPostal -> Ciudad` es semánticamente válida: un código postal siempre se asocia a una única ciudad.
*   La DF `Ciudad -> CódigoPostal` **no** es semánticamente válida: una ciudad puede tener múltiples códigos postales. Si en tu base de datos actual cada ciudad solo tiene un código postal, es una coincidencia, no una regla semántica. Un diseño que aplicara `Ciudad -> CódigoPostal` como una DF sería incorrecto y podría generar problemas futuros.

---
**Enlaces:**
- [[20260111160000-modelizacion-temporal-de-datos]]
