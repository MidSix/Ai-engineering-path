# Test de Autoevaluación - Tema 2: Introducción a TCP/IP

Este test contiene preguntas de opción múltiple diseñadas para evaluar la comprensión de los conceptos de la arquitectura TCP/IP, direccionamiento y puertos, siguiendo un estilo práctico y basado en escenarios.

---

### Pregunta 1

Un PC con la IP `192.168.1.50` quiere enviar un paquete a un servidor web en la IP `203.0.113.80`. El PC está en una red doméstica detrás de un router. El router tiene la IP pública `85.30.1.100`. Cuando el paquete sale del router hacia Internet, ¿qué direcciones IP de origen y destino contendrá la cabecera IP?

*   a) Origen: `192.168.1.50`, Destino: `203.0.113.80`
*   b) Origen: `85.30.1.100`, Destino: `203.0.113.80`
*   c) Origen: `192.168.1.1` (IP del router en la LAN), Destino: `203.0.113.80`
*   d) Origen: `85.30.1.100`, Destino: `85.30.1.100`

<details>
  <summary>Ver Respuesta</summary>
  
  **Respuesta correcta: b)**
  
  **Justificación:** Este es el funcionamiento de NAT (Network Address Translation). Las direcciones IP privadas como `192.168.1.50` no son enrutables en Internet. El router debe traducir la dirección de origen privada a su propia dirección pública (`85.30.1.100`) para que el servidor web pueda responder. La dirección de destino final no se modifica.
</details>

---

### Pregunta 2

Tu ordenador abre una conexión a un servidor web (`http://example.com`). Tu sistema operativo le asigna a tu navegador el puerto de origen TCP `54123`. ¿Cuáles son los puertos de origen y destino en el primer segmento TCP (el SYN) que tu ordenador envía al servidor?

*   a) Puerto Origen: `80`, Puerto Destino: `54123`
*   b) Puerto Origen: `54123`, Puerto Destino: `80`
*   c) Puerto Origen: `54123`, Puerto Destino: `54123`
*   d) Puerto Origen: `80`, Puerto Destino: `80`

<details>
  <summary>Ver Respuesta</summary>
  
  **Respuesta correcta: b)**
  
  **Justificación:** El cliente (tu navegador) utiliza un puerto efímero o dinámico (superior a 1023, en este caso `54123`) como puerto de origen. El servidor web atiende las peticiones HTTP en su puerto bien conocido (well-known port), que es el `80`. Por lo tanto, la conexión se establece desde el puerto `54123` del cliente hacia el puerto `80` del servidor.
</details>

---

### Pregunta 3

Una dirección IP de Clase A, como `10.0.0.0`, fue diseñada originalmente para un propósito específico. ¿Cuál de las siguientes afirmaciones describe mejor la estructura de una dirección de Clase A?

*   a) Permitía un gran número de redes, pero cada red podía tener muy pocos hosts.
*   b) Permitía un número muy limitado de redes, pero cada red podía tener millones de hosts.
*   c) Equilibraba el número de redes y de hosts, permitiendo una cantidad moderada de ambos.
*   d) Estaba reservada exclusivamente para tráfico multicast.

<details>
  <summary>Ver Respuesta</summary>
  
  **Respuesta correcta: b)**
  
  **Justificación:** En el sistema de direccionamiento por clases, las direcciones de Clase A utilizaban los primeros 8 bits (1 byte) para el identificador de red y los 24 bits restantes (3 bytes) para el identificador de host. Esto resultaba en un número muy pequeño de redes de Clase A posibles, pero cada una de ellas podía albergar 2^24 (más de 16 millones) de hosts, haciéndolas adecuadas para organizaciones gigantescas.
</details>

---

### Pregunta 4

Un host `A` quiere enviar un paquete IP al host `B`, que está en su misma red Ethernet. El host `A` conoce la dirección IP de `B`, pero su caché ARP está vacía. ¿Qué protocolo y qué dirección de destino usará el host `A` para poder enviar finalmente el paquete a `B`?

*   a) Enviará una petición DNS con la dirección de destino `255.255.255.255`.
*   b) Enviará el paquete IP directamente usando la dirección MAC de destino `FF:FF:FF:FF:FF:FF`.
*   c) Enviará una petición ICMP Echo Request al router para que le informe de la MAC de B.
*   d) Enviará una petición ARP con la dirección MAC de destino `FF:FF:FF:FF:FF:FF` para descubrir la MAC de B.

<details>
  <summary>Ver Respuesta</summary>
  
  **Respuesta correcta: d)**
  
  **Justificación:** Para enviar un paquete en una red local, la capa de enlace necesita conocer la dirección MAC de destino. Si no está en la caché, el protocolo ARP (Address Resolution Protocol) se encarga de descubrirla. Lo hace enviando una petición ARP (un broadcast, con MAC de destino `FF:FF:FF:FF:FF:FF`) a toda la red, preguntando "¿quién tiene esta dirección IP?". El host `B` responderá con su dirección MAC.
</details>

---

### Pregunta 5

Analizando el tráfico de una red, vemos un paquete UDP y un paquete TCP. ¿Qué función realizan ambos protocolos obligatoriamente?

*   a) Ambos garantizan que los datos lleguen en orden.
*   b) Ambos realizan multiplexación/demultiplexación usando números de puerto.
*   c) Ambos establecen una conexión antes de enviar datos.
*   d) Ambos retransmiten los paquetes perdidos.

<details>
  <summary>Ver Respuesta</summary>
  
  **Respuesta correcta: b)**
  
  **Justificación:** La función esencial y común de la capa de transporte, tanto en TCP como en UDP, es la multiplexación. Esto permite que múltiples aplicaciones en un host puedan usar la red simultáneamente. La dirección IP dirige el paquete al host correcto, y el número de puerto lo dirige a la aplicación correcta dentro de ese host. El resto de las opciones (entrega ordenada, conexión, retransmisiones) son características de TCP, pero no de UDP.
</details>
