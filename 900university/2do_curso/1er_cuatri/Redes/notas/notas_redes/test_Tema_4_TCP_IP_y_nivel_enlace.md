# Test de Autoevaluación - Tema 4: TCP/IP y el Nivel de Enlace

Este test contiene preguntas de opción múltiple diseñadas para evaluar la comprensión de la interacción entre la capa de red (IP) y la capa de enlace, con un enfoque especial en el protocolo ARP.

---

### Pregunta 1

Un host `A` (IP: 192.168.0.10) quiere enviar un paquete a un host `B` (IP: 192.168.0.20) que se encuentra en su misma red local Ethernet. La caché ARP del host `A` está vacía. ¿Cuál es el primer paso que debe realizar `A` para poder enviar el paquete IP?

*   a) Enviar el paquete IP con la dirección MAC de destino `FF:FF:FF:FF:FF:FF`, asumiendo que `B` lo recogerá.
*   b) Enviar una trama de broadcast con una petición ARP (ARP Request) preguntando por la dirección MAC asociada a la IP `192.168.0.20`.
*   c) Contactar al servidor DNS para que traduzca la dirección IP de `B` a una dirección MAC.
*   d) Enviar el paquete al router por defecto para que este se encargue de encontrar al host `B`.

<details>
  <summary>Ver Respuesta</summary>
  
  **Respuesta correcta: b)** 
  
  **Justificación:** Para construir una trama de capa de enlace válida, el host de origen necesita conocer la dirección MAC del host de destino (si está en la misma red). Si esta información no está en su caché ARP, utiliza el Protocolo de Resolución de Direcciones (ARP). Emite un ARP Request a la dirección de broadcast de la capa de enlace (`FF:FF:FF:FF:FF:FF`), que es recibido por todos los nodos en la red local. Solo el host que posee la IP solicitada (`B`) responderá con un ARP Reply conteniendo su dirección MAC.
</details>

---

### Pregunta 2

Siguiendo con el escenario anterior, el host `B` recibe la petición ARP de `A`. ¿Qué información clave contiene la respuesta ARP (ARP Reply) que `B` envía de vuelta a `A`?

*   a) La dirección IP de `B` y la dirección MAC de `A`.
*   b) La dirección MAC de `B` y la dirección MAC de broadcast.
*   c) La dirección IP de `B` y la dirección MAC de `B`. La respuesta se envía como unicast a la dirección MAC de `A`.
*   d) La dirección IP del router y la dirección MAC de `B`.

<details>
  <summary>Ver Respuesta</summary>
  
  **Respuesta correcta: c)** 
  
  **Justificación:** El propósito del ARP Reply es informar al solicitante de la dirección MAC correspondiente a la IP que se buscaba. La respuesta contiene tanto la IP (`192.168.0.20`) como la MAC del host que responde (`B`). A diferencia de la petición, la respuesta es una trama de unicast, ya que se envía directamente a la dirección MAC del host que originó la petición (`A`), la cual se aprendió del propio ARP Request.
</details>

---

### Pregunta 3

Una dirección MAC (Media Access Control) se compone de 48 bits. ¿Qué representan las dos mitades de esta dirección?

*   a) Los primeros 24 bits identifican la red y los últimos 24 bits identifican al host.
*   b) Los primeros 24 bits son el OUI (Organizationally Unique Identifier) que identifica al fabricante, y los últimos 24 bits son un número de serie asignado por el fabricante.
*   c) Los primeros 24 bits son un número aleatorio y los últimos 24 bits indican la velocidad de la tarjeta de red.
*   d) La dirección completa es asignada dinámicamente por el protocolo DHCP cada vez que el dispositivo se conecta a la red.

<details>
  <summary>Ver Respuesta</summary>
  
  **Respuesta correcta: b)** 
  
  **Justificación:** Las direcciones MAC están diseñadas para ser únicas a nivel mundial. La IEEE asigna bloques de identificadores de 24 bits (OUI) a cada fabricante de hardware. El fabricante, a su vez, asigna los 24 bits restantes de forma única a cada dispositivo que produce, garantizando (en teoría) que no haya dos tarjetas de red en el mundo con la misma dirección MAC.
</details>

---

### Pregunta 4

Un host `C` (IP: 10.0.0.5) quiere comunicarse con un servidor web en Internet (IP: 8.8.8.8). El host `C` está en una LAN con el router `R` (IP: 10.0.0.1). ¿Qué dirección MAC de destino usará el host `C` en las tramas Ethernet que contienen los paquetes IP destinados al servidor web?

*   a) La dirección MAC del servidor web (8.8.8.8), que la buscará con ARP.
*   b) La dirección MAC del router `R`.
*   c) La dirección de broadcast `FF:FF:FF:FF:FF:FF`, ya que el destino está fuera de la red.
*   d) La dirección MAC del propio host `C`, para que el paquete le sea devuelto si falla.

<details>
  <summary>Ver Respuesta</summary>
  
  **Respuesta correcta: b)**
  
  **Justificación:** El host `C` sabe (por su tabla de enrutamiento) que la IP de destino `8.8.8.8` no está en su red local. Por lo tanto, debe enviar los paquetes a su puerta de enlace por defecto (el router `R`) para que este los reenvíe hacia Internet. A nivel de la capa de enlace, la comunicación es salto a salto. El "siguiente salto" para el host `C` es el router `R`. Por ello, `C` encapsulará el paquete IP en una trama Ethernet con la dirección MAC de destino del router `R` (que obtendrá vía ARP si no la conoce ya).
</details>

---

### Pregunta 5

¿Cuál es la función principal de la caché ARP en un sistema operativo?

*   a) Almacenar las páginas web visitadas con más frecuencia para acelerar la carga.
*   b) Guardar una correspondencia temporal de las direcciones IP y las direcciones MAC recientemente descubiertas para evitar enviar peticiones ARP repetitivas.
*   c) Mantener un registro de todas las direcciones IP a las que el host se ha comunicado en su historia.
*   d) Almacenar las claves de cifrado de las redes WiFi a las que se ha conectado.

<details>
  <summary>Ver Respuesta</summary>
  
  **Respuesta correcta: b)**
  
  **Justificación:** Las peticiones ARP, al ser broadcasts, consumen recursos de red y CPU en todos los dispositivos de la LAN. Para optimizar este proceso, los sistemas operativos mantienen una caché (una tabla temporal en memoria) con las asignaciones IP-MAC que han aprendido recientemente. Antes de enviar un ARP Request, el sistema consulta esta caché. Si encuentra una entrada válida, utiliza la dirección MAC almacenada y se ahorra el proceso de descubrimiento por broadcast, mejorando significativamente la eficiencia de la red.
</details>
