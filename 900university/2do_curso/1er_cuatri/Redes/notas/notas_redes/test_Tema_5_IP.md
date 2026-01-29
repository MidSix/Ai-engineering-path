# Test de Autoevaluación - Tema 5: IP (Internet Protocol)

Este test contiene preguntas de opción múltiple diseñadas para evaluar la comprensión de los conceptos clave de la capa de Red, incluyendo el direccionamiento IP, la fragmentación, subredes, DHCP y NAT.

---

### Pregunta 1

Un router recibe un datagrama IP de 4000 bytes. La red por la que debe reenviar este datagrama tiene una MTU (Maximum Transmission Unit) de 1500 bytes. El datagrama tiene el bit "Don't Fragment" (DF) a 0. ¿Qué hará el router?

*   a) Descartará el datagrama y enviará un mensaje ICMP "Destination Unreachable" al origen.
*   b) Fragmentará el datagrama en tres fragmentos más pequeños, cada uno con su propia cabecera IP, y los enviará secuencialmente.
*   c) Enviará el datagrama completo, ignorando la MTU de la red.
*   d) Le pedirá al host de origen que le envíe el datagrama de nuevo en trozos más pequeños.

<details>
  <summary>Ver Respuesta</summary>
  
  **Respuesta correcta: b)** 
  
  **Justificación:** Cuando un datagrama IP es más grande que la MTU de la red de salida y el bit DF no está activado, el router tiene la obligación de fragmentarlo. La carga útil del datagrama original se dividirá en trozos que, junto con la cabecera IP (que se copia en cada fragmento con ligeras modificaciones en los campos de fragmentación), no superen la MTU. En este caso, 4000 bytes (20 de cabecera + 3980 de datos) se dividirán, por ejemplo, en tres fragmentos para no superar los 1500 bytes.
</details>

---

### Pregunta 2

Te han asignado el bloque de direcciones de red `192.168.10.0/24`. Necesitas crear 4 subredes para cuatro departamentos distintos. ¿Cuál de las siguientes máscaras de subred te permitiría cumplir este requisito?

*   a) `255.255.255.0` (/24)
*   b) `255.255.255.128` (/25)
*   c) `255.255.255.192` (/26)
*   d) `255.255.255.240` (/28)

<details>
  <summary>Ver Respuesta</summary>
  
  **Respuesta correcta: c)** 
  
  **Justificación:** Partimos de una máscara de 24 bits. Para crear 4 subredes, necesitamos tomar prestados 'n' bits de la porción de host, de tal forma que 2^n >= 4. El menor 'n' que cumple esto es 2 (2^2 = 4). Por lo tanto, necesitamos añadir 2 bits a la máscara de red original, resultando en una máscara de 24 + 2 = 26 bits. Una máscara de /26 en decimal es `255.255.255.192` (ya que 128 + 64 = 192).
</details>

---

### Pregunta 3

Un ordenador se conecta a una red y envía un mensaje `DHCP Discover`. ¿Qué está intentando hacer el ordenador y cómo envía este mensaje inicial?

*   a) Está intentando encontrar al router para salir a Internet, enviando un mensaje unicast.
*   b) Está buscando un servidor DHCP para obtener automáticamente una configuración de red (IP, máscara, gateway). Envía el mensaje a la dirección de broadcast `255.255.255.255`.
*   c) Ya tiene una IP y está anunciando su presencia en la red con un mensaje de broadcast.
*   d) Está intentando resolver un nombre de dominio (como google.com) a una dirección IP.

<details>
  <summary>Ver Respuesta</summary>
  
  **Respuesta correcta: b)** 
  
  **Justificación:** El proceso DHCP (Dynamic Host Configuration Protocol) comienza cuando un cliente, que aún no tiene dirección IP, necesita obtener una. Para ello, envía un mensaje `DHCP Discover` para localizar servidores DHCP en la red. Como no conoce la IP de ningún servidor (y ni siquiera tiene IP propia), este mensaje inicial se envía como un broadcast a nivel de IP (`255.255.255.255`) y a nivel de MAC (`FF:FF:FF:FF:FF:FF`).
</details>

---

### Pregunta 4

Un usuario en su casa tiene una red con la dirección `192.168.1.0/24`. Su ordenador (IP: `192.168.1.100`) se conecta a un servidor en Internet (IP: `203.0.113.10`). El router de su casa tiene la IP pública `80.58.61.250`. Cuando el paquete llega al servidor de Internet, ¿qué dirección IP de origen ve el servidor?

*   a) `192.168.1.100`
*   b) `192.168.1.1` (la IP interna del router)
*   c) `80.58.61.250`
*   d) `255.255.255.255`

<details>
  <summary>Ver Respuesta</summary>
  
  **Respuesta correcta: c)**
  
  **Justificación:** Este es el funcionamiento básico de NAT (Network Address Translation), específicamente PAT/NAPT. Las direcciones IP del rango `192.168.0.0/16` son privadas y no se pueden enrutar en Internet. Cuando el paquete del ordenador sale del router hacia Internet, el router reescribe la cabecera IP, cambiando la dirección IP de origen (la privada, `192.168.1.100`) por su propia dirección IP pública (`80.58.61.250`). Guarda esta traducción en una tabla para poder dirigir la respuesta de vuelta al ordenador correcto.
</details>

---

### Pregunta 5

En la cabecera de un datagrama IPv4, ¿cuál es el propósito del campo TTL (Time To Live)?

*   a) Mide el tiempo en segundos que un paquete ha estado viajando por la red.
*   b) Asegura que el paquete sea entregado en un tiempo determinado.
*   c) Evita que los paquetes entren en bucles de enrutamiento indefinidamente, limitando el número de saltos (routers) que pueden atravesar.
*   d) Indica la prioridad del paquete para que los routers lo procesen más rápido.

<details>
  <summary>Ver Respuesta</summary>
  
  **Respuesta correcta: c)**
  
  **Justificación:** El TTL es un campo de 8 bits que se inicializa en el host de origen (comúnmente con valores como 64 o 128). Cada router por el que pasa el paquete decrementa el valor del TTL en al menos una unidad. Si un router recibe un paquete con un TTL de 1, lo decrementa a 0, descarta el paquete y envía un mensaje ICMP "Time Exceeded" al origen. Esto es un mecanismo de seguridad crucial para prevenir que paquetes perdidos o mal enrutados circulen eternamente por la red, consumiendo recursos.
</details>
