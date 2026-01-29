# Memoria Entrelazada (Memory Interleaving)

#tags: memoria, arquitectura, rendimiento

La **memoria entrelazada** es una técnica de organización de la memoria principal que la divide en varios módulos independientes (llamados bancos de memoria). El objetivo es aumentar el ancho de banda efectivo del sistema de memoria, permitiendo accesos simultáneos a diferentes módulos. En lugar de esperar a que un único y gran banco de memoria termine una operación, la CPU puede iniciar operaciones en paralelo con varios bancos, siempre que las direcciones solicitadas residan en módulos distintos.

El rendimiento del sistema depende crucialmente de cómo se distribuyen las direcciones de memoria entre los módulos. Existen dos estrategias principales:

---
## 1. Entrelazado de Orden Superior (High-Order Interleaving)

En esta configuración, los bits de mayor peso (más significativos) de la dirección de memoria se utilizan para seleccionar el módulo. Los bits de menor peso se usan para seleccionar la palabra dentro de ese módulo.

- **Mecanismo:** `Módulo = f(bits_altos)`, `Offset = f(bits_bajos)`
- **Ejemplo de cálculo:** `Módulo = floor(Dirección / Tamaño_Módulo)`
- **Efecto:** Las direcciones consecutivas de memoria se ubican dentro del mismo módulo. Por ejemplo, en un sistema con módulos de 8 palabras, las direcciones de 0 a 7 estarán en el Módulo 0, de 8 a 15 en el Módulo 1, y así sucesivamente.
- **Ventajas:** Proporciona un buen aislamiento. Un fallo en un módulo no afecta a los otros y es fácil asignar módulos completos a procesos o dispositivos específicos.
- **Desventajas:** Es ineficaz para el acceso a datos contiguos (como arrays o vectores), ya que todas las solicitudes recaen secuencialmente sobre el mismo módulo, perdiendo la oportunidad de paralelizar.

---
## 2. Entrelazado de Orden Inferior (Low-Order Interleaving)

Es la técnica más común. Utiliza los bits de menor peso (menos significativos) de la dirección para seleccionar el módulo de memoria.

- **Mecanismo:** `Módulo = f(bits_bajos)`, `Offset = f(bits_altos)`
- **Ejemplo de cálculo:** `Módulo = Dirección % Número_de_Módulos`
- **Efecto:** Las direcciones consecutivas de memoria se reparten ("entrelazan") entre los diferentes módulos. La dirección 0 va al Módulo 0, la 1 al Módulo 1, la 2 al Módulo 2, etc.
- **Ventajas:** Maximiza el paralelismo para accesos secuenciales. Cuando un programa lee un bloque de datos contiguo, las solicitudes se distribuyen uniformemente entre todos los módulos, permitiendo un acceso mucho más rápido.
- **Desventajas:** Un fallo en un módulo puede afectar a todo el sistema, ya que las direcciones están repartidas por todas partes.

---
## Ejemplo Práctico

Para ver una aplicación directa de estos dos métodos y cómo se calcula el rendimiento en cada caso, puedes consultar la solución del siguiente ejercicio:

- [[practica4_1_ejercicio6.md]]
