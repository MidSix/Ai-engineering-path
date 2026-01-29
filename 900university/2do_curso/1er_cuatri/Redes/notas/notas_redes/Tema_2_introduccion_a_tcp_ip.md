# Notas Detalladas sobre el Tema 2: Introducción a TCP/IP

Este documento expande los conceptos presentados en las diapositivas del "Tema 2", proporcionando explicaciones más profundas sobre la arquitectura TCP/IP, el direccionamiento y los protocolos clave.

---

### **1. La Arquitectura TCP/IP: Capas y Protocolos**

Las diapositivas muestran un modelo de 5 capas, que es una forma práctica de ver la arquitectura de Internet. Es importante entender su relación con el modelo teórico OSI (7 capas) y el modelo formal TCP/IP (4 capas).

| Modelo OSI (Teórico) | Modelo TCP/IP (5 Capas, Práctico) | Modelo TCP/IP (4 Capas, Formal) | Propósito Principal                                                            |
| :------------------- | :-------------------------------- | :------------------------------ | :----------------------------------------------------------------------------- |
| Aplicación           | **Aplicación**                    | Aplicación                      | Proporciona servicios de red a las aplicaciones del usuario (HTTP, SMTP).      |
| Presentación         |                                   |                                 | Traduce y formatea los datos (cifrado, compresión).                            |
| Sesión               |                                   |                                 | Gestiona las sesiones de comunicación entre aplicaciones.                      |
| **Transporte**       | **Transporte**                    | **Transporte**                  | Comunicación **extremo a extremo** entre *procesos* (aplicaciones).            |
| **Red**              | **Red**                           | **Internet**                    | Comunicación **salto a salto** entre *hosts* (ordenadores) a través de la red. |
| **Enlace**           | **Enlace**                        | Acceso a la Red                 | Comunicación entre nodos en el *mismo* enlace físico.                          |
| **Físico**           | **Físico**                        |                                 | Transmisión de bits a través del medio físico.                                 |

#### **¿Por qué Tantas Capas?**

La pregunta en la diapositiva 4 es clave: "¿Para qué necesito dos niveles más intermedios?"

*   **Capa de Red (IP):** Su trabajo es llevar un paquete desde un ordenador de origen a un ordenador de destino, sin importar si están en la misma habitación o en continentes diferentes. Es como el servicio de correos, que puede llevar una carta de una casa a otra. A esto se le llama comunicación **salto a salto** (*hop-by-hop*), porque los routers van pasando el paquete de uno a otro hasta llegar al destino.
*   **Capa de Transporte (TCP/UDP):** La capa de red sabe cómo llegar al ordenador correcto, pero ¿cómo sabe a qué aplicación entregar el paquete? Un ordenador puede estar ejecutando un navegador web, un cliente de correo y un juego a la vez. La capa de transporte se encarga de esto usando **números de puerto**. Un puerto identifica un proceso o aplicación específica. Esta capa proporciona comunicación **extremo a extremo** (*end-to-end*), creando un canal lógico directamente entre la aplicación cliente y la aplicación servidor.

---

### **2. Encapsulación: El Viaje de los Datos**

La diapositiva 8 muestra el proceso de encapsulación. Es la idea central del funcionamiento de las redes de capas.

**Analogía:** Enviar una carta de un departamento a otro en diferentes empresas.

1.  **Capa de Aplicación:** Escribes un mensaje (ej: "Hola"). Esto son los **Datos**.
2.  **Capa de Transporte:** Tomas los datos y los metes en un sobre. En el sobre escribes los **puertos** de origen y destino (el número de tu departamento y el del departamento de destino). Este sobre es el **Segmento TCP** o **Datagrama UDP**.
3.  **Capa de Red:** Metes el sobre anterior en otro sobre más grande. En este escribes las **direcciones IP** de origen y destino (la dirección de tu edificio y la del edificio de destino). Este nuevo sobre es el **Paquete IP**.
4.  **Capa de Enlace:** Le das el paquete al servicio de mensajería de tu edificio. Ellos lo meten en una valija de reparto y le ponen la **dirección MAC** del siguiente punto (el router de la esquina). Esta valija es la **Trama Ethernet**.

El proceso inverso en el destino se llama **desencapsulación** o **demultiplexión**. Cada capa abre su sobre, lee la información de su cabecera y pasa el contenido a la capa superior correspondiente.

**Protocolos Auxiliares:**

*   **ARP (Address Resolution Protocol):** Es el "traductor" entre la Capa 3 y la Capa 2. Cuando la capa IP sabe que tiene que enviar un paquete a la IP `192.168.1.50`, pregunta a la red: "**¿Quién tiene la IP `192.168.1.50`? ¡Dime tu dirección MAC!**". El dispositivo correspondiente responde, y así la capa de enlace sabe a qué dirección física enviar la trama.
*   **ICMP (Internet Control Message Protocol):** Es un protocolo de ayuda para la capa de red. No transporta datos de usuario, sino mensajes de control y error. El comando `ping` utiliza ICMP para comprobar si un host está activo.

---

### **3. Direccionamiento IP: Identificadores Únicos**

Una dirección IP es un identificador lógico para una interfaz de red.

#### **Clases de IP (El Sistema Antiguo)**

Originalmente, las direcciones IPv4 se dividían en clases. Esto determinaba qué parte de la dirección era el **Identificador de Red** y qué parte era el **Identificador de Host**.

*   **Clase A (empieza en `0...`):** 1 byte para red, 3 para hosts. `N.H.H.H`. Permitía pocas redes, pero con millones de hosts cada una (ideal para gigantes como IBM).
*   **Clase B (empieza en `10...`):** 2 bytes para red, 2 para hosts. `N.N.H.H`. Un equilibrio entre número de redes y hosts (ideal para universidades, grandes empresas).
*   **Clase C (empieza en `110...`):** 3 bytes para red, 1 para host. `N.N.N.H`. Permitía millones de redes, pero con solo 254 hosts cada una (ideal para redes pequeñas).

Este sistema era muy rígido y desperdiciaba muchas direcciones. Por ejemplo, si necesitabas una red para 300 hosts, tenías que pedir un bloque de Clase B (con 65,534 hosts), desperdiciando la gran mayoría. Esto llevó a la creación de **subredes** y, finalmente, a **CIDR (Classless Inter-Domain Routing)**, el sistema que se usa hoy y que permite tamaños de red flexibles.

#### **Direcciones Públicas vs. Privadas y NAT**

*   **Direcciones Públicas:** Son únicas a nivel mundial y se asignan por organismos oficiales. Son "visibles" y accesibles desde cualquier punto de Internet.
*   **Direcciones Privadas:** Son rangos reservados para uso interno en redes locales (LANs). **No son enrutables en Internet**. Millones de hogares y oficinas usan el mismo rango de IPs privadas (ej. `192.168.1.0/24`) sin que haya conflictos.
    *   Clase A: `10.0.0.0` – `10.255.255.255`
    *   Clase B: `172.16.0.0` – `172.31.255.255`
    *   Clase C: `192.168.0.0` – `192.168.255.255`

*   **NAT (Network Address Translation):** ¿Cómo accede a Internet un dispositivo con una IP privada? Gracias a NAT, una función que suele realizar el router.
    *   **Funcionamiento:** Cuando tu PC (`192.168.1.50`) quiere conectarse a `google.com`, envía el paquete al router. El router reemplaza la IP de origen privada (`192.168.1.50`) por su propia **IP pública** (la que le da tu proveedor de Internet). Cuando la respuesta de Google vuelve al router, este consulta su tabla NAT, ve que esa respuesta era para `192.168.1.50`, y le reenvía el paquete. Actúa como un recepcionista que gestiona toda la correspondencia externa de una oficina.

---

### **4. Tipos de Comunicación: Unicast, Broadcast, Multicast**

*   **Unicast (Uno a Uno):** La comunicación más común. Un paquete va desde un origen único a un destino único.
*   **Broadcast (Uno a Todos):** Un paquete se envía a todos los dispositivos en la misma red local. Es como gritar en una habitación: todos te oyen, lo quieran o no. Esto consume muchos recursos, por lo que los routers no suelen reenviar los paquetes de broadcast fuera de la red local.
*   **Multicast (Uno a Muchos):** Una solución intermedia y más eficiente. Un origen envía un único paquete a una "dirección de grupo" especial. Solo los hosts que se han "suscrito" a ese grupo recibirán y procesarán el paquete. Es como un programa de radio: el locutor emite una vez, y solo quienes han sintonizado esa emisora lo escuchan. Se usa mucho para la distribución de vídeo en directo (IPTV).

---

### **5. Números de Puerto: Las Puertas de las Aplicaciones**

Si la dirección IP es la dirección de un edificio, el número de puerto es el número del apartamento.

*   **Función:** Es un identificador de 16 bits (del 1 al 65535) que permite a la capa de transporte saber a qué aplicación o proceso entregar un segmento de datos.
*   **Puertos de Servidor (Well-Known Ports: 1-1023):** Son puertos estándar y fijos para servicios conocidos. Un servidor web siempre "escucha" en el puerto 80 (HTTP) o 443 (HTTPS).
*   **Puertos de Cliente (Puertos Efímeros: 49152-65535):** Cuando tu navegador quiere conectarse a un servidor web, el sistema operativo le asigna un puerto de origen temporal y aleatorio de este rango (ej. puerto 51234). La comunicación sería:
    *   **Petición:** `[Tu IP]:51234` → `[IP de Google]:443`
    *   **Respuesta:** `[IP de Google]:443` → `[Tu IP]:51234`

Así, tu ordenador puede tener múltiples pestañas del navegador abiertas (cada una usando un puerto efímero diferente) conectándose al mismo o a diferentes servidores web.
