# Test de Autoevaluación - Tema 3: Tecnologías del Nivel de Enlace

Este test contiene preguntas de opción múltiple diseñadas para evaluar la comprensión de los conceptos fundamentales del Tema 3, incluyendo Ethernet, CSMA/CD, direcciones MAC y tecnologías WiFi.

---

### Pregunta 1

En una red Ethernet clásica que utiliza CSMA/CD, dos estaciones, A y C, intentan transmitir una trama exactamente en el mismo instante. ¿Qué suceso ocurrirá en la red?

*   a) Se producirá una colisión, y ambas estaciones detendrán su transmisión inmediatamente, esperarán un tiempo aleatorio definido por el algoritmo de *exponential backoff* y volverán a intentar transmitir.
*   b) La estación con la dirección MAC más baja (alfabéticamente) ganará el arbitraje y su trama se transmitirá correctamente, mientras que la otra deberá esperar.
*   c) Ambas tramas se entregarán correctamente, ya que los switches modernos pueden manejar múltiples transmisiones simultáneas.
*   d) Se producirá una colisión, pero las estaciones completarán la transmisión de sus tramas para luego reenviarlas.

<details>
  <summary>Ver Respuesta</summary>
  
  **Respuesta correcta: a)** 
  
  **Justificación:** El mecanismo CSMA/CD (Carrier Sense Multiple Access with Collision Detection) dicta que si se detecta una colisión, las estaciones implicadas deben dejar de transmitir, enviar una señal de "jamming" para asegurar que todos los nodos son conscientes de la colisión, y luego cada una espera un periodo de tiempo aleatorio (que se duplica en cada intento fallido) antes de volver a escuchar el medio para retransmitir.
</details>

---

### Pregunta 2

Un administrador de red está configurando un nuevo switch. Conecta tres ordenadores a los puertos 1, 2 y 3. Inicialmente, la tabla de direcciones MAC del switch está vacía. El ordenador en el puerto 1 (MAC: 0A:0A:0A:0A:0A:0A) envía una trama destinada al ordenador en el puerto 3 (MAC: 0C:0C:0C:0C:0C:0C). ¿Cómo procesará el switch esta primera trama?

*   a) El switch descartará la trama porque no conoce la MAC de destino.
*   b) El switch enviará la trama únicamente al puerto 3.
*   c) El switch enviará la trama a todos sus puertos, excepto al puerto 1 (de origen). Al mismo tiempo, aprenderá que la MAC 0A:0A:0A:0A:0A:0A está en el puerto 1.
*   d) El switch enviará una petición ARP para descubrir en qué puerto se encuentra la MAC de destino.

<details>
  <summary>Ver Respuesta</summary>
  
  **Respuesta correcta: c)** 
  
  **Justificación:** Un switch, al recibir una trama con una dirección de destino que no está en su tabla MAC (un "unicast flooding" o inundación de unicast), la reenvía por todos sus puertos excepto por el que la recibió. Simultáneamente, el switch inspecciona la dirección MAC de origen de la trama y la asocia en su tabla con el puerto por el que llegó, en este caso, asociando 0A:0A:0A:0A:0A:0A al puerto 1. Entonces la trama es enviada a TODOS los puertos menos por el que llego, los hosts reciben la trama y comparan la MAC destino con la suya, si no coinciden descartan la trama, si coinciden la aceptan. Y aunque la tabla MAC del switch no este llena, si la MAC destino no esta en su tabla pues simplemente hace flooding, envia la trama POR TODOS los puertos menos por el que entro.
</details>

---

### Pregunta 3

Una trama Ethernet tiene la siguiente dirección MAC de destino: `FF:FF:FF:FF:FF:FF`. ¿Qué tipo de trama es y quiénes la procesarán en la red local?

*   a) Es una trama de unicast destinada a un servidor específico.
*   b) Es una trama de multicast destinada a un grupo concreto de dispositivos.
*   c) Es una trama corrupta y será descartada por el primer switch que la reciba.
*   d) Es una trama de broadcast y será procesada por todos los dispositivos en el mismo dominio de broadcast.

<details>
  <summary>Ver Respuesta</summary>
  
  **Respuesta correcta: d)** 
  
  **Justificación:** La dirección MAC `FF:FF:FF:FF:FF:FF` es la dirección de broadcast estándar para la capa de enlace. Las tramas enviadas a esta dirección son recibidas y procesadas por todas las interfaces de red activas dentro del mismo dominio de broadcast (típicamente, la misma LAN o VLAN). Es utilizada por protocolos como ARP para descubrir direcciones.
</details>

---

### Pregunta 4

En una red WiFi, el "problema del nodo oculto" impide que CSMA/CD sea efectivo. ¿Qué mecanismo utiliza el estándar 802.11 (WiFi) para mitigar este problema antes de enviar tramas de datos grandes?

*   a) El uso de claves de cifrado WPA3 para que los nodos no puedan interferirse.
*   b) El mecanismo de sondeo (polling), donde el punto de acceso asigna turnos de transmisión a cada estación.
*   c) El intercambio de tramas RTS (Request to Send) y CTS (Clear to Send) entre la estación y el punto de acceso.
*   d) Simplemente se basa en la suerte y retransmite las tramas si no reciben un ACK.

<details>
  <summary>Ver Respuesta</summary>
  
  **Respuesta correcta: c)**
  
  **Justificación:** Para evitar colisiones de nodos que no se "escuchan" entre sí pero que sí son escuchados por el punto de acceso (el problema del nodo oculto), se implementa CSMA/CA (Collision Avoidance). Opcionalmente, para tramas grandes, una estación puede enviar un `RTS` al punto de acceso. El punto de acceso responde con un `CTS` que es escuchado por todos los nodos en su rango, informándoles que deben permanecer en silencio durante un tiempo determinado, reservando así el medio para la estación original.
</details>

---

### Pregunta 5

¿Cuál es la principal diferencia entre WEP (Wired Equivalent Privacy) y los estándares de seguridad más modernos como WPA2/WPA3?

*   a) WEP utiliza claves de 256 bits, mientras que WPA2 solo usa claves de 128 bits.
*   b) WEP es más seguro porque utiliza un cifrado estático que no cambia.
*   c) WEP utiliza una clave estática y un vector de inicialización (IV) corto y reutilizado, lo que lo hace vulnerable a ataques prácticos. WPA2/WPA3 utilizan claves dinámicas y protocolos robustos como CCMP/AES.
*   d) No hay diferencias significativas; WPA es solo un nombre comercial para versiones más nuevas de WEP.

<details>
  <summary>Ver Respuesta</summary>
  
  **Respuesta correcta: c)**
  
  **Justificación:** La debilidad fundamental de WEP reside en su uso de una única clave estática y compartida, junto con un IV (Vector de Inicialización) de solo 24 bits que se reutiliza con frecuencia. Esto permite a un atacante capturar tráfico y, en poco tiempo, deducir la clave de red. WPA2 y WPA3 solucionaron esto introduciendo protocolos (TKIP y, principalmente, CCMP/AES) que gestionan claves de forma dinámica para cada sesión y cada paquete, eliminando las vulnerabilidades críticas de WEP.
</details>
