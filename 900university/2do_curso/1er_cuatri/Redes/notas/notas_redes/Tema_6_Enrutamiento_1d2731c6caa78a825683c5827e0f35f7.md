# Notas Detalladas sobre el Tema 6: Enrutamiento e ICMP

Este documento expande los conceptos presentados en las diapositivas del "Tema 6", proporcionando explicaciones más profundas sobre el funcionamiento del enrutamiento IP, las tablas de enrutamiento y el rol diagnóstico de ICMP.

---

### **1. El Proceso de Enrutamiento: Salto a Salto (Hop-by-Hop)**

Internet se basa en el principio de enrutamiento **salto a salto**. Ningún dispositivo conoce la ruta completa desde el origen hasta el destino. En su lugar, cada router (cada "salto") toma una decisión independiente para acercar el paquete un paso más a su destino.

*   **Rol del Host (PC, servidor, etc.):** La lógica de un host es simple. Cuando quiere enviar un paquete, se pregunta: "¿La IP de destino está en mi misma red local?".
    *   **Sí:** Lo envía directamente al host de destino (usando ARP para encontrar su MAC).
    *   **No:** Lo envía a su **puerta de enlace por defecto (default gateway)**, que es la dirección IP del router local. El host confía en que ese router sabrá qué hacer con el paquete.

*   **Rol del Router:** Un router es un dispositivo más inteligente, conectado a múltiples redes. Su trabajo es reenviar paquetes entre ellas. Cuando un router recibe un paquete:
    1.  Examina la dirección IP de destino del paquete.
    2.  Consulta su **tabla de enrutamiento** para decidir cuál de sus interfaces de salida es la mejor para enviar el paquete.
    3.  Reenvía el paquete al **siguiente salto** (el siguiente router en el camino).

Este proceso se repite en cada router del camino hasta que el paquete llega a un router que está directamente conectado a la red de destino final.

---

### **2. La Tabla de Enrutamiento y el Algoritmo de Búsqueda**

La tabla de enrutamiento es el "cerebro" de cualquier dispositivo en la capa de red. Es una lista de reglas que le dice al dispositivo cómo reenviar paquetes. Cada entrada tiene, como mínimo, esta información:

*   **Destino:** La dirección de la red o subred de destino.
*   **Máscara:** La máscara de subred para esa red de destino.
*   **Gateway (o Siguiente Salto):** La dirección IP del próximo router al que se debe enviar el paquete. Si la red está directamente conectada, este campo puede ser `0.0.0.0`.
*   **Interfaz:** La interfaz de red local (ej. `eth0`, `eth1`) que se debe usar para enviar el paquete.

#### **El Algoritmo de la "Coincidencia de Prefijo Más Larga" (Longest Prefix Match)**

Cuando un dispositivo necesita enrutar un paquete, sigue un algoritmo estricto:

1.  Se toma la dirección IP de destino del paquete.
2.  El dispositivo recorre **toda** su tabla de enrutamiento y busca **todas** las entradas que coincidan. Una entrada coincide si:
    `(IP de Destino del Paquete) AND (Máscara de la Entrada) == (Destino de la Entrada)`
3.  De todas las entradas que han coincidido, el dispositivo selecciona **la que tenga la máscara de subred más larga** (es decir, con más bits a 1). Esta es la ruta "más específica".
4.  Una vez seleccionada la mejor ruta, el paquete se envía al `Gateway` indicado a través de la `Interfaz` indicada.

**Ejemplo:**
Un router recibe un paquete para `195.30.25.10`. Su tabla tiene estas dos entradas:
*   `195.30.0.0/16` (Máscara `255.255.0.0`)
*   `195.30.25.0/24` (Máscara `255.255.255.0`)

Ambas rutas coinciden. Sin embargo, la segunda (`/24`) tiene una máscara más larga que la primera (`/16`), por lo que es más específica. El router elegirá la segunda ruta.

**La Ruta por Defecto (`0.0.0.0/0`):**
Esta es una ruta especial que coincide con **cualquier** dirección IP. Sin embargo, su máscara tiene una longitud de 0, por lo que **siempre perderá** contra cualquier otra ruta más específica que también coincida. Actúa como el último recurso: "Si no tienes una ruta mejor para este paquete, envíamelo a mí".

---

### **3. Enrutamiento Estático vs. Dinámico**

*   **Enrutamiento Estático:**
    *   **Cómo funciona:** Un administrador de red configura manualmente cada una de las rutas en cada router.
    *   **Ventajas:** Muy seguro (no hay protocolos que puedan ser atacados), predecible y no consume recursos de CPU/red en los routers.
    *   **Desventajas:** Es completamente manual y no escala. Si un enlace falla, el tráfico no se redirige automáticamente; el administrador tiene que intervenir. Es inviable para redes grandes y solo se usa en redes muy pequeñas y estables o para configurar rutas específicas (como la ruta por defecto).

*   **Enrutamiento Dinámico:**
    *   **Cómo funciona:** Los routers ejecutan **protocolos de enrutamiento** para comunicarse entre sí. Se anuncian las redes que conocen y "aprenden" la topología de la red de forma automática.
    *   **Ventajas:** Automático y escalable. Si un router o un enlace falla, los protocolos lo detectan y recalculan nuevas rutas para sortear el problema. Así es como funciona Internet.
    *   **Desventajas:** Consume más recursos del router y es conceptualmente más complejo.
    *   **Ejemplos de protocolos:** RIP, OSPF, BGP.

---

### **4. CIDR y Superredes: Agregación de Rutas**

El sistema de direccionamiento por clases (A, B, C) era muy rígido. **CIDR (Classless Inter-Domain Routing)** lo reemplazó, permitiendo máscaras de subred de longitud variable.

Una consecuencia directa de CIDR es la capacidad de hacer **supernetting** o **agregación de rutas**, que es lo contrario de subnetting.

*   **El Problema:** Imagina que un proveedor de Internet (ISP) necesita 4000 direcciones IP. No puede usar una red de Clase B (demasiado grande) y una de Clase C (demasiado pequeña). La solución sería asignarle 16 redes de Clase C consecutivas. El problema es que esto obligaría a todos los routers de Internet a tener 16 entradas en sus tablas para ese ISP. Si esto se repite para miles de ISPs, las tablas de enrutamiento globales crecerían hasta ser inmanejables.

*   **La Solución (Supernetting):** El ISP puede tomar esas 16 redes de Clase C consecutivas (ej. desde `194.10.160.0/24` hasta `194.10.175.0/24`) y anunciarlas al mundo como una única ruta agregada: **`194.10.160.0/20`**.
    *   Una máscara `/20` deja 12 bits para hosts, lo que permite 4096 direcciones, cubriendo las 16 redes de clase C (`16 * 256 = 4096`).
    *   Ahora, los routers de Internet solo necesitan **una entrada** para saber cómo llegar a cualquiera de esas 4000 direcciones. Esto es vital para mantener el tamaño de las tablas de enrutamiento de Internet bajo control.

---

### **5. ICMP: El Sistema de Diagnóstico de IP**

IP no tiene mecanismos de error; simplemente descarta paquetes. **ICMP (Internet Control Message Protocol)** es su compañero, un protocolo de ayuda que se usa para reportar errores y hacer consultas de diagnóstico. Los mensajes ICMP viajan dentro de paquetes IP.

#### **Mensajes ICMP Clave:**

*   **Echo Request (Type 8) y Echo Reply (Type 0):**
    *   **Uso:** La base de la herramienta `ping`.
    *   **Funcionamiento:** Un host envía un Echo Request a un destino. Si el destino está activo y lo recibe, responde con un Echo Reply. Esto confirma la conectividad bidireccional.

*   **Destination Unreachable (Type 3):**
    *   **Uso:** Un router o host informa de que no puede entregar un paquete.
    *   **Subtipos comunes:**
        *   **Network Unreachable:** Un router no tiene ninguna ruta en su tabla para la red de destino.
        *   **Host Unreachable:** Un router está en la red de destino, pero no puede encontrar al host específico (ej. una petición ARP falla).
        *   **Port Unreachable:** El paquete llega correctamente al host de destino, pero no hay ninguna aplicación escuchando en el puerto TCP/UDP de destino.

*   **Time Exceeded (Type 11):**
    *   **Uso:** La base de la herramienta `traceroute`.
    *   **Funcionamiento:** Un router ha recibido un paquete, ha decrementado su TTL a 0 y, por lo tanto, lo ha descartado. Envía este mensaje de vuelta al emisor original para informarle.

*   **Redirect (Type 5):**
    *   **Uso:** Optimización de rutas en una red local.
    *   **Funcionamiento:** Un host envía un paquete a su router por defecto (R1). R1 mira el destino y se da cuenta de que hay un router mejor (R2) en la misma LAN para llegar a ese destino. R1 reenvía el paquete a R2, pero también envía un ICMP Redirect al host original diciéndole: "Para la próxima vez, envía los paquetes para este destino directamente a R2".

#### **Traceroute: El Mapa de la Ruta**

La herramienta `traceroute` (o `tracert` en Windows) es un ingenioso uso de los mensajes ICMP para descubrir los "saltos" (routers) en el camino hacia un destino.

1.  El programa `traceroute` envía un paquete UDP a un puerto inválido del host destino, pero con un **TTL = 1**.
2.  El primer router en el camino recibe el paquete, decrementa el TTL a 0, descarta el paquete y envía un **ICMP Time Exceeded** de vuelta. ¡`traceroute` acaba de descubrir la IP del primer router!
3.  Luego, envía otro paquete igual, pero con **TTL = 2**. Este paquete atraviesa el primer router (que lo deja con TTL=1), pero es descartado por el segundo router, que envía de vuelta su propio ICMP Time Exceeded. ¡`traceroute` ya conoce la IP del segundo router!
4.  Este proceso se repite, incrementando el TTL en 1 cada vez, para descubrir cada salto sucesivo en la ruta.
5.  Finalmente, un paquete con un TTL suficientemente alto llega al host de destino. El host intenta entregar el paquete al puerto UDP inválido. Como no hay ninguna aplicación escuchando, el SO del destino responde con un **ICMP Port Unreachable**.
6.  Cuando el programa `traceroute` recibe este mensaje en lugar de un "Time Exceeded", sabe que ha llegado al final del trayecto y el trazado de la ruta está completo.
