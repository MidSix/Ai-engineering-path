# Notas Detalladas sobre el Tema 5: El Protocolo de Internet (IP)

Este documento expande los conceptos presentados en las diapositivas del "Tema 5", proporcionando explicaciones más profundas sobre el servicio IP, la cabecera IPv4, el subnetting, DHCP y NAT.

---

### **1. El Servicio IP: "Mejor Esfuerzo" (Best-Effort)**

La diapositiva 3 establece que IP es **no fiable** y **no orientado a conexión**. Esto define su filosofía de diseño fundamental.

*   **No Orientado a Conexión:** IP trata cada paquete (datagrama) como una entidad completamente independiente. No establece una conexión o un circuito previo. Cada paquete puede, en teoría, seguir una ruta diferente a través de Internet para llegar al mismo destino.
*   **No Fiable:** IP no ofrece ninguna garantía.
    *   **No garantiza la entrega:** Un router puede descartar un paquete si está congestionado o si el TTL del paquete expira.
    *   **No garantiza el orden:** Dos paquetes enviados desde A hacia B pueden tomar rutas distintas, y el segundo paquete podría llegar antes que el primero.
    *   **No garantiza la integridad:** No hay un mecanismo robusto en la cabecera IP para verificar que los *datos* del paquete no se hayan corrompido (el checksum solo cubre la cabecera).

Este modelo de **"mejor esfuerzo"** significa que IP hace todo lo posible por entregar los paquetes, pero si algo sale mal, simplemente los descarta. No realiza retransmisiones ni gestión de errores compleja.

**¿Por qué este diseño?**
Esta simplicidad hace que la capa de red sea extremadamente rápida y robusta. Mantiene a los routers (el núcleo de la red) simples, ya que no necesitan mantener estado de las conexiones. La responsabilidad de la fiabilidad se delega a la capa superior (Capa de Transporte), específicamente a **TCP**, que se encarga de reordenar los paquetes, solicitar retransmisiones y garantizar una entrega fiable cuando la aplicación lo necesita.

---

### **2. La Cabecera IPv4: El Mapa de Ruta del Paquete**

La cabecera IP contiene toda la información que un router necesita para reenviar un paquete. Vamos a detallar los campos más importantes:

*   **Versión (4 bits):** Indica la versión del protocolo. Para IPv4, siempre es `4` (binario `0100`).
*   **Longitud de Cabecera (IHL, 4 bits):** Indica la longitud de la cabecera en *palabras de 32 bits*. Normalmente es `5` (5 x 32 bits = 160 bits = 20 bytes), pero puede ser mayor si hay opciones.
*   **Longitud Total (16 bits):** La longitud total del datagrama (cabecera + datos) en bytes. El tamaño máximo es 65,535 bytes.
*   **TTL (Time-To-Live, 8 bits):** Un contador de saltos que evita que los paquetes vaguen por Internet indefinidamente. El emisor lo suele fijar en un valor como 64 o 128. **Cada router que procesa el paquete decrementa el TTL en 1**. Si un router recibe un paquete con TTL=1, lo decrementa a 0, lo descarta y envía un mensaje de error ICMP al origen.
*   **Protocolo (8 bits):** Indica a qué protocolo de la capa de transporte deben entregarse los datos en el destino. Es la clave de demultiplexación de la capa de red.
    *   `6` = TCP
    *   `17` = UDP
    *   `1` = ICMP
*   **Checksum de Cabecera (16 bits):** Una suma de comprobación para detectar errores **solo en la cabecera**. Se recalcula en cada router, ya que el campo TTL cambia. Si un router detecta un error de checksum, descarta el paquete.
*   **Direcciones IP Origen y Destino (32 bits cada una):** Las direcciones lógicas del emisor y el receptor final.

#### **La Fragmentación en Detalle**

Los campos **Identificación**, **Flags** y **Offset de Fragmentación** se usan para dividir un paquete grande en trozos más pequeños.

*   **El Problema (MTU):** Cada tecnología de red tiene un **MTU (Maximum Transmission Unit)**, el tamaño máximo de paquete que puede transportar. Por ejemplo, el MTU de Ethernet es de 1500 bytes. Si un router recibe un paquete de 1500 bytes y tiene que enviarlo por un enlace con un MTU de 500 bytes, no puede enviarlo entero.
*   **La Solución:** El router fragmenta el paquete grande en tres fragmentos más pequeños.
    1.  Copia la cabecera IP original en los tres fragmentos.
    2.  Asigna a los tres fragmentos el mismo número de **Identificación** para que el receptor sepa que pertenecen al mismo paquete original.
    3.  Usa el campo **Offset de Fragmentación** para indicar la posición de cada fragmento dentro del paquete original.
    4.  En los dos primeros fragmentos, activa el flag **"Más Fragmentos" (MF)**. En el último fragmento, este flag va a 0 para indicar que es el final.
*   El reensamblado de los fragmentos lo realiza **siempre el host de destino final**, nunca los routers intermedios.

---

### **3. Subredes: Jerarquía y Organización**

Como se explica en las diapositivas, tener una red "plana" es ineficiente. El **subnetting** o la creación de subredes permite a un administrador de red tomar un único bloque de direcciones grande y dividirlo en varios segmentos de red más pequeños.

*   **Beneficios:**
    *   **Reducción del tráfico de broadcast:** Un broadcast enviado en una subred no se propaga a las otras.
    *   **Gestión de red simplificada:** Se pueden aplicar políticas de seguridad por subred.
    *   **Enrutamiento eficiente:** El router principal de la empresa solo necesita saber cómo llegar a las subredes, no a cada host individual.

#### **La Máscara de Subred y la Notación CIDR**

La **máscara de subred** es la herramienta que define qué parte de una dirección IP corresponde a la red/subred y qué parte al host.

*   Una máscara es un número de 32 bits. Los `1`s representan la porción de red+subred, y los `0`s la porción de host.
*   **Notación CIDR (Classless Inter-Domain Routing):** Es la forma moderna de expresar la máscara. Una dirección como `210.53.23.65/26` significa que los primeros **26 bits** son para la red y la subred, y los restantes `32 - 26 = 6` bits son para identificar a los hosts dentro de esa subred.

**Cálculo Clave:** Para obtener la dirección de una subred (su identificador), se realiza una operación `AND` a nivel de bits entre la dirección IP y la máscara de subred.

#### **FLSM vs. VLSM**

*   **FLSM (Fixed Length Subnet Mask):** Todas las subredes que se crean tienen el mismo tamaño. Es fácil de calcular, pero muy ineficiente. Si divides una red de Clase C en 4 subredes, cada una tendrá 62 hosts usables. Si un departamento solo necesita 10 IPs y otro 50, se desperdician muchas direcciones en el primero.
*   **VLSM (Variable Length Subnet Mask):** Es la técnica moderna y eficiente. Permite dividir el espacio de direcciones en subredes de **diferentes tamaños** según las necesidades. La estrategia es asignar primero las subredes más grandes (las que necesitan más hosts y, por tanto, una máscara más corta) y luego ir asignando las más pequeñas con los bloques restantes.

---

### **4. DHCP: Configuración Automática de Hosts**

**DHCP (Dynamic Host Configuration Protocol)** es un protocolo de cliente-servidor que automatiza la asignación de direcciones IP y otros parámetros de red. Sin él, tendríamos que configurar manualmente cada dispositivo.

El proceso de 4 pasos se conoce como **DORA**:

1.  **Discover:** El cliente se conecta a la red. Como no tiene IP, envía un mensaje **DHCPDISCOVER** a la dirección de broadcast (`255.255.255.255`). El mensaje dice: *"Soy nuevo aquí, mi MAC es X. ¿Hay algún servidor DHCP que pueda darme una IP?"*.
2.  **Offer:** Los servidores DHCP que reciben el mensaje responden con un **DHCPOFFER**. El mensaje dice: *"Hola, MAC X. Te ofrezco la IP `192.168.1.102`, la máscara `255.255.255.0`, el router `192.168.1.1` y el DNS `8.8.8.8` por un período de 24 horas."*.
3.  **Request:** El cliente recibe una o más ofertas. Elige una (normalmente la primera que llega) y envía un **DHCPREQUEST** por broadcast. El mensaje dice: *"Acepto la oferta de la IP `192.168.1.102` del servidor Y."*. Se envía por broadcast para que cualquier otro servidor que haya hecho una oferta sepa que ha sido rechazada y puede liberar la IP que había ofrecido.
4.  **Acknowledge:** El servidor DHCP elegido registra la asignación en su base de datos y envía un **DHCPACK** final al cliente, confirmando la concesión. El cliente ya está configurado y puede empezar a comunicarse.

**APIPA (Automatic Private IP Addressing):** Si un cliente está configurado para usar DHCP pero no recibe respuesta de ningún servidor, se autoasigna una dirección en el rango **`169.254.0.0/16`**. Esto no le da acceso a Internet, pero le permite comunicarse con otros dispositivos en la misma LAN que también estén en esa situación.

---

### **5. NAT: El Traductor de Direcciones**

**NAT (Network Address Translation)** es la tecnología que permite que múltiples dispositivos en una red privada compartan una única dirección IP pública para acceder a Internet. Su principal motivación fue combatir el agotamiento de las direcciones IPv4.

El tipo más común es **PAT (Port Address Translation) o NAPT**.

**Funcionamiento:**

1.  Un PC en la red privada, `172.25.25.15`, quiere conectarse a `google.com`. Elige un puerto de origen efímero, por ejemplo, `12345`.
2.  Envía el paquete (`Origen=172.25.25.15:12345`, `Destino=209.85.129.99:80`) a su router.
3.  El router NAT recibe el paquete. Su IP pública es `200.19.18.17`.
4.  Crea una entrada en su **tabla de traducciones NAT**:
    | IP LAN | Puerto LAN | Puerto WAN |
    | :--- | :--- | :--- |
    | `172.25.25.15` | `12345` | `30123` (un puerto que elige él) |
5.  Modifica el paquete saliente:
    *   **IP Origen:** `200.19.18.17` (su IP pública)
    *   **Puerto Origen:** `30123` (el puerto que eligió)
6.  Cuando la respuesta de Google vuelve al router (`Destino=200.19.18.17:30123`), el router consulta su tabla NAT.
7.  Encuentra la entrada: "Ah, el puerto `30123` corresponde a `172.25.25.15` en el puerto `12345`".
8.  Vuelve a modificar el paquete para que la IP y puerto de destino sean `172.25.25.15:12345` y lo reenvía a la red interna.

**Ventajas:**
*   Ahorra direcciones IPv4.
*   Proporciona seguridad básica: por defecto, no se permiten conexiones iniciadas desde el exterior hacia la red privada, ya que el router no tendría una entrada en su tabla para saber a quién reenviar ese tráfico no solicitado.

**Inconvenientes:**
*   Rompe el principio de conectividad extremo a extremo de Internet.
*   Crea problemas para aplicaciones P2P o servidores alojados en casa que necesitan recibir conexiones entrantes. Esto se suele solucionar con **NAT Traversal** o configurando **Port Forwarding** en el router.
