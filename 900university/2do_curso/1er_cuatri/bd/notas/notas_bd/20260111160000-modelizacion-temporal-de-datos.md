# 20260111160000 Modelización Temporal de Datos

**Etiquetas:** #bases_de_datos #modelo_ER #temporal #histórico #anomalías

## Concepto

La modelización temporal de datos se enfoca en almacenar y gestionar información a lo largo del tiempo, evitando la "destrucción" de datos que ocurre con las operaciones `UPDATE` o `DELETE` tradicionales. El objetivo es mantener un histórico completo de los cambios.

Se utilizan dos conceptos de tiempo principales:
- **Tiempo de Validez (`tvi`, `tvf`):** El intervalo (`[tvi, tvf)`) durante el cual un dato es cierto en el mundo real. Un `tvf` nulo indica que el dato sigue siendo válido "hasta nuevo aviso".
- **Tiempo de Transacción:** El intervalo durante el cual el dato estuvo registrado en la base de datos (menos usado en modelado conceptual).

## Estrategias de Modelado (y sus Anomalías)

Existen principalmente tres estrategias para modelar la temporalidad en un diagrama E/R.

### 1. Histórico por Atributo/Relación
- **Descripción:** Se crea una entidad débil histórica por cada atributo o relación que varía en el tiempo.
- **Anomalías/Inconvenientes:**
    - **Proliferación de tablas:** Genera un número muy elevado de tablas históricas.
    - **Consultas complejas:** Recuperar el estado completo de una entidad en un punto del tiempo requiere muchos `JOINs`.

### 2. Histórico Único
- **Descripción:** Se crea una única entidad débil histórica que agrupa todos los atributos y relaciones que pueden variar.
- **Anomalías/Inconvenientes:**
    - **Redundancia:** Si solo cambia un atributo, se debe crear un nuevo registro repitiendo todos los demás atributos que no cambiaron.
    - **Granularidad única:** Fuerza a usar la misma granularidad de tiempo (ej. días) para todos los atributos, aunque unos cambien con menos frecuencia que otros.

### 3. Imagen Especular (Mirror Image)
- **Descripción:** La entidad principal siempre contiene el estado actual. Se crea una entidad débil "espejo" que guarda una copia completa de cada estado pasado.
- **Anomalías/Inconvenientes:**
    - **Máxima redundancia:** El histórico duplica toda la información, incluso la que nunca cambia (ej. `DNI`, `Nombre`).
    - **Gestión doble:** Requiere una lógica cuidadosa para mantener la tabla actual y la histórica.

## Operaciones y Consultas en un Histórico

### Gestión de Cambios (Principio de "No Borrar")

Para actualizar un dato (ej: un cambio de dirección):
1.  **Cerrar el registro actual:** Se busca el registro con `tvf` nulo y se actualiza su `tvf` a la fecha justo antes de que el cambio entre en vigor.
2.  **Insertar el nuevo registro:** Se crea un nuevo registro con la información actualizada, un `tvi` igual a la fecha del cambio y un `tvf` nulo.

### El Rol del `tvi` como Discriminador

En una entidad débil histórica, un mismo propietario tiene múltiples registros. El `tvi` (tiempo de validez inicial) sirve para distinguir unívocamente cada una de esas versiones históricas. La clave primaria de la entidad débil es, por tanto, `{Clave_Propietario_FK, tvi}`.

### Consultar el Estado Activo

Para encontrar el registro o estado actual de una entidad, se busca la versión cuyo `tvf` es `NULL`.

```sql
-- Ejemplo para obtener el estado actual de un empleado
SELECT * FROM HISTORICO_EMPLEADO
WHERE ID_Empleado_FK = 123 AND tvf IS NULL;
```

---
**Enlaces:**
- [[20260109130510-dominio-en-modelo-relacional]]
