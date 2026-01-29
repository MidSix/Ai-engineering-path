# Mega Test Final de Redes - Nivel Examen

Este test contiene 40 preguntas diseñadas para ser un desafío significativo, cubriendo los 11 temas de la asignatura. Las preguntas se centran en escenarios complejos, casos límite y detalles técnicos para poner a prueba un conocimiento profundo de la materia.

---

**1. En una red de conmutación de paquetes que utiliza datagramas, se envían dos paquetes (P1 y P2) desde un Host A a un Host B a través de una serie de routers. Si P1 se envía 10ms antes que P2, ¿qué se puede afirmar con certeza sobre su llegada al Host B?**

- a) P1 siempre llegará antes que P2, ya que fueron enviados en ese orden.
- b) P1 y P2 llegarán con una diferencia de 10ms, manteniendo el intervalo de envío original.
- c) P2 podría llegar antes que P1, o incluso no llegar, mientras que P1 sí llega.
- d) Si P1 se pierde, P2 también se perderá, ya que siguen la misma ruta.

<details>
<summary>Ver Respuesta</summary>

**Respuesta correcta: c)**

**Explicación:** En una red de datagramas (como Internet, que usa IP), cada paquete se enruta de forma independiente. Esto significa que P1 y P2 pueden seguir rutas diferentes. Una ruta puede congestionarse o ser más larga, provocando que P2 llegue antes que P1, que P1 se descarte por TTL o congestión, etc. No hay garantía de orden ni de entrega.

</details>

---

**2. ¿Cuál es la diferencia fundamental entre el retardo de transmisión y el retardo de propagación?**

- a) El retardo de transmisión depende de la longitud del enlace, mientras que el de propagación depende del tamaño del paquete.
- b) El retardo de transmisión es el tiempo que tarda un bit en viajar por el medio, y el de propagación es el tiempo en que el paquete espera en la cola del router.
- c) El retardo de transmisión es el tiempo necesario para empujar todos los bits del paquete al enlace, mientras que el de propagación es el tiempo que tarda un bit en viajar desde un extremo del enlace al otro.
- d) Son dos términos para el mismo concepto: el tiempo total que tarda un paquete en cruzar un enlace.

<details>
<summary>Ver Respuesta</summary>

**Respuesta correcta: c)**

**Explicación:** El retardo de transmisión depende del tamaño del paquete y del ancho de banda del enlace (Tamaño / Ancho de Banda). Es el tiempo que se tarda en "poner" el paquete en el "cable". El retardo de propagación depende de la distancia física del enlace y de la velocidad de la luz en ese medio (Distancia / Velocidad de propagación). Es el tiempo que tarda el primer bit en llegar al otro lado una vez está en el cable.

</details>

---

**3. En el modelo OSI, ¿qué capa es la responsable de asegurar que los datos se entregan libres de errores, en secuencia, y sin pérdidas ni duplicaciones entre dos sistemas finales?**

- a) La capa de Red, que gestiona el enrutamiento global.
- b) La capa de Enlace de Datos, que gestiona la transmisión en un solo salto.
- c) La capa de Transporte, que proporciona un servicio extremo a extremo.
- d) La capa de Sesión, que gestiona el diálogo entre aplicaciones.

<details>
<summary>Ver Respuesta</summary>

**Respuesta correcta: c)**

**Explicación:** La capa de Transporte (Capa 4) es la que ofrece servicios de comunicación extremo a extremo. Protocolos como TCP, que operan en esta capa, proporcionan fiabilidad, control de flujo y control de errores, asegurando que la comunicación entre las aplicaciones de los hosts finales sea correcta. La capa de enlace solo se preocupa por un salto, y la de red (IP) ofrece un servicio "best-effort" no fiable.

</details>

---

**4. ¿Cuál de los siguientes dispositivos opera principalmente en la Capa 2 (Enlace de Datos) del modelo OSI y toma decisiones de reenvío basadas en direcciones MAC?**

- a) Un Hub (Concentrador).
- b) Un Router.
- c) Un Switch (Conmutador).
- d) Un Repetidor.

<details>
<summary>Ver Respuesta</summary>

**Respuesta correcta: c)**

**Explicación:** Un Switch opera en la Capa 2. Mantiene una tabla de direcciones MAC para saber a qué puerto reenviar las tramas, evitando colisiones y segmentando la red. Un Hub o Repetidor opera en Capa 1 (Física), simplemente regenerando y enviando la señal a todos sus puertos. Un Router opera en Capa 3 (Red), usando direcciones IP para enrutar paquetes entre distintas redes.

</details>

---

**5. En una trama Ethernet II, el campo "Tipo" tiene el valor `0x0806`. ¿Qué significa este valor?**

- a) El protocolo de la capa superior es IPv4.
- b) El protocolo de la capa superior es ARP.
- c) El protocolo de la capa superior es IPv6.
- d) La trama es una trama de control para VLANs (802.1Q).

<details>
<summary>Ver Respuesta</summary>

**Respuesta correcta: b)**

**Explicación:** El campo "EtherType" o "Tipo" en una trama Ethernet II indica el protocolo de capa de red encapsulado en la carga útil de la trama. El valor `0x0800` corresponde a IPv4, mientras que `0x0806` corresponde a ARP (Address Resolution Protocol). IPv6 usa el valor `0x86DD`.

</details>

---

**6. ¿En qué escenario el mecanismo CSMA/CD de Ethernet es completamente ineficaz para evitar colisiones?**

- a) En una red con un solo hub y múltiples estaciones.
- b) En una red full-duplex conectada por un switch.
- c) En una red inalámbrica (Wi-Fi).
- d) Cuando dos estaciones están en los extremos opuestos de un cable coaxial muy largo.

<details>
<summary>Ver Respuesta</summary>

**Respuesta correcta: c)**

**Explicación:** CSMA/CD (Carrier Sense Multiple Access with Collision Detection) requiere que una estación pueda "escuchar" el medio mientras transmite para detectar si otra estación ha transmitido a la vez (colisión). En redes inalámbricas, debido al "problema del nodo oculto" y a que una estación no puede escuchar y transmitir en la misma frecuencia simultáneamente de forma efectiva, no se puede detectar colisiones. Por eso, las redes Wi-Fi usan CSMA/CA (Collision Avoidance).

</details>

---

**7. Un host A con IP `192.168.1.10` y MAC `AA:AA:AA:AA:AA:AA` quiere enviar un paquete IP al host B con IP `192.168.2.20`. Están en subredes diferentes y el router por defecto de A es `192.168.1.1` con MAC `RR:RR:RR:RR:RR:RR`. ¿Qué direcciones MAC de origen y destino tendrá la trama Ethernet que salga del host A?**

- a) Origen: `AA:AA:..`, Destino: `FF:FF:FF:FF:FF:FF` (Broadcast)
- b) Origen: `AA:AA:..`, Destino: MAC del Host B (que se descubrirá)
- c) Origen: `AA:AA:..`, Destino: `RR:RR:..`
- d) Origen: `RR:RR:..`, Destino: MAC del Host B

<details>
<summary>Ver Respuesta</summary>

**Respuesta correcta: c)**

**Explicación:** La comunicación entre subredes distintas debe pasar por un router. El host A sabe que la IP de destino no está en su red local. Por lo tanto, el paquete IP (con IP origen A e IP destino B) debe ser enviado al router por defecto para que este lo reenvíe. A nivel de enlace, la comunicación es un solo salto: del host A al router. Así, la trama Ethernet tendrá la MAC de A como origen y la MAC del router (`RR:RR..`) como destino. El router, al recibirla, creará una nueva trama para el siguiente salto.

</details>

---

**8. ¿Cuál es el propósito principal del protocolo ARP (Address Resolution Protocol)?**

- a) Asignar dinámicamente direcciones IP a los hosts de una red.
- b) Traducir nombres de dominio (como `www.google.com`) a direcciones IP.
- c) Obtener la dirección MAC correspondiente a una dirección IP conocida dentro de la misma red local.
- d) Encontrar la mejor ruta para enviar un paquete a un destino en otra red.

<details>
<summary>Ver Respuesta</summary>

**Respuesta correcta: c)**

**Explicación:** Cuando un host necesita enviar una trama a otro host en la misma LAN, necesita conocer la dirección MAC de destino para construir la cabecera de la trama. Si solo conoce la dirección IP de destino, utiliza ARP. Envía una petición ARP (ARP Request) por broadcast preguntando "¿quién tiene esta IP?". El host con esa IP responde con una respuesta ARP (ARP Reply) que contiene su dirección MAC.

</details>

---

**9. En la cabecera de un datagrama IPv4, ¿qué sucede si el valor del campo TTL (Time to Live) llega a cero?**

- a) El paquete es devuelto inmediatamente a su origen con un mensaje de error.
- b) El router que decrementó el TTL a cero descarta el paquete y puede enviar un mensaje ICMP "Tiempo excedido" al origen.
- c) El paquete se marca como de baja prioridad y se sigue enrutando.
- d) El router pide al origen que retransmita el paquete con un TTL mayor.

<details>
<summary>Ver Respuesta</summary>

**Respuesta correcta: b)**

**Explicación:** El campo TTL es un mecanismo para evitar que los paquetes vaguen indefinidamente por la red en caso de bucles de enrutamiento. Cada router por el que pasa el paquete decrementa el TTL en uno. Si un router lo recibe con TTL=1, lo decrementa a 0, lo descarta y, opcionalmente, envía un mensaje ICMP de tipo "Tiempo excedido" a la dirección IP de origen para notificar el problema. La herramienta `traceroute` se basa en este comportamiento.

</details>

---

**10. Una red tiene la dirección `172.20.0.0/22`. ¿Cuál de las siguientes afirmaciones es correcta?**

- a) La máscara de subred es `255.255.255.252`.
- b) La primera dirección IP utilizable para un host es `172.20.0.1` y la última es `172.20.3.254`.
- c) Hay `22` bits disponibles para identificar hosts.
- d) Esta red no puede ser subdividida en subredes más pequeñas.

<details>
<summary>Ver Respuesta</summary>

**Respuesta correcta: b)**

**Explicación:** La notación `/22` significa que los primeros 22 bits se usan para la red y los `32 - 22 = 10` bits restantes para los hosts. La máscara de subred es `255.255.252.0`. El rango de IPs para esta red va desde `172.20.0.0` hasta `172.20.3.255`. La primera IP (`172.20.0.0`) es la dirección de red y la última (`172.20.3.255`) es la de broadcast. Por lo tanto, el rango de hosts válidos es de `172.20.0.1` a `172.20.3.254`.

</details>

---

**11. ¿Cuál es el principal propósito del protocolo DHCP (Dynamic Host Configuration Protocol)?**

- a) Resolver direcciones MAC a partir de direcciones IP.
- b) Permitir que un host configure automáticamente su interfaz de red (IP, máscara, gateway, DNS).
- c) Traducir direcciones IP privadas a una única dirección IP pública.
- d) Controlar el flujo de datos en una conexión TCP.

<details>
<summary>Ver Respuesta</summary>

**Respuesta correcta: b)**

**Explicación:** DHCP automatiza el proceso de configuración de red para los clientes. Un cliente nuevo en la red puede enviar un mensaje `DHCPDISCOVER` para encontrar un servidor DHCP. El servidor responde con un `DHCPOFFER` proponiendo una configuración (IP, máscara, etc.). El cliente la solicita con un `DHCPREQUEST` y el servidor la confirma con un `DHCPACK`. Este proceso se conoce como DORA.

</details>

---

**12. Un router NAT recibe un paquete saliente de una red privada con IP origen `192.168.0.50` y puerto origen `50000`. La IP pública del router es `80.90.100.110`. ¿Qué modificación fundamental realiza el router NAT/PAT al paquete antes de enviarlo a Internet?**

- a) Cambia solo la dirección IP de origen a `80.90.100.110`.
- b) Reemplaza la IP origen por `80.90.100.110` y el puerto origen por un puerto efímero de su elección (p.ej. `15000`), registrando esta traducción en su tabla.
- c) Cifra la carga útil del paquete para proteger los datos privados.
- d) Añade una cabecera adicional que contiene la IP privada original.

<details>
<summary>Ver Respuesta</summary>

**Respuesta correcta: b)**

**Explicación:** Esto describe el funcionamiento de PAT (Port Address Translation) o NAPT, la forma más común de NAT. Para permitir que múltiples dispositivos internos usen la misma IP pública, el router no solo traduce la IP, sino también el puerto de origen. Crea una entrada en su tabla de traducción que asocia `(192.168.0.50:50000)` con `(80.90.100.110:15000)`. Cuando llegue la respuesta a `80.90.100.110:15000`, el router usará la tabla para revertir la traducción y enviar el paquete al host interno correcto.

</details>

---

**13. En el algoritmo de enrutamiento de estado de enlace (Link-State), como OSPF, ¿qué información intercambian los routers entre sí?**

- a) Su tabla de enrutamiento completa.
- b) Únicamente la distancia (número de saltos) a todos los destinos conocidos.
- c) Información sobre sus enlaces directamente conectados y el estado de los mismos (coste).
- d) Paquetes de "hola" para confirmar que los vecinos están activos, sin más datos de topología.

<details>
<summary>Ver Respuesta</summary>

**Respuesta correcta: c)**

**Explicación:** En los algoritmos de estado de enlace, cada router construye un mapa completo de la topología de la red. Para ello, cada router "inunda" la red con paquetes LSA (Link-State Advertisement) que contienen información sobre sus vecinos directos y el coste para alcanzarlos. Todos los routers reciben todos los LSAs y construyen la misma vista de la red, sobre la cual ejecutan un algoritmo (como Dijkstra) para calcular las mejores rutas. Esto contrasta con los algoritmos de vector-distancia (como RIP) donde los routers solo intercambian sus tablas de enrutamiento con sus vecinos directos.

</details>

---

**14. ¿Qué es el "longest match prefix" en el contexto del enrutamiento IP?**

- a) La ruta con el mayor número de saltos para llegar al destino.
- b) Si un destino coincide con varias entradas en una tabla de enrutamiento, se elige la entrada con la máscara de subred más específica (con más bits a 1).
- c) La ruta que ha estado en la tabla de enrutamiento durante más tiempo.
- d) Un algoritmo que prioriza las rutas que usan direcciones IP de mayor valor numérico.

<details>
<summary>Ver Respuesta</summary>

**Respuesta correcta: b)**

**Explicación:** Es un principio fundamental del enrutamiento IP. Si una dirección IP de destino, por ejemplo `192.168.1.100`, coincide con dos entradas en la tabla, como `192.168.1.0/24` y `192.168.1.64/26`, el router elegirá la segunda (`/26`) porque es más específica (tiene una máscara de red más larga). Esto permite crear rutas más detalladas para subconjuntos de una red mayor. La ruta por defecto (`0.0.0.0/0`) es el "prefix" más corto posible y solo se usa si no hay ninguna otra coincidencia.

</details>

---

**15. ¿Para qué se utiliza `traceroute` (o `tracert` en Windows)?**

- a) Para medir el ancho de banda disponible entre dos puntos de la red.
- b) Para descubrir la ruta (la secuencia de routers) que siguen los paquetes hacia un host de destino.
- c) Para resolver el nombre de dominio de una dirección IP.
- d) Para mostrar todas las conexiones TCP activas en un host.

<details>
<summary>Ver Respuesta</summary>

**Respuesta correcta: b)**

**Explicación:** `traceroute` funciona enviando paquetes (UDP, ICMP o TCP) hacia el destino con un valor de TTL incremental. El primer paquete se envía con TTL=1, el primer router lo descarta y responde con un ICMP "Tiempo excedido", revelando su IP. El segundo se envía con TTL=2, el segundo router responde, y así sucesivamente. De esta forma, se traza la ruta salto a salto hasta llegar al destino.

</details>

---

**16. Un servidor recibe un datagrama UDP en un puerto en el que ningún proceso está escuchando. ¿Cuál es la respuesta esperada del servidor?**

- a) El servidor no responde y descarta el paquete silenciosamente.
- b) El servidor responde con un segmento TCP con el flag RST activado.
- c) El servidor responde con un mensaje ICMP "Puerto Inalcanzable".
- d) El servidor mantiene el datagrama en un buffer por si algún proceso abre ese puerto pronto.

<details>
<summary>Ver Respuesta</summary>

**Respuesta correcta: c)**

**Explicación:** El servicio UDP es mínimo. Cuando la capa de transporte del sistema operativo recibe un datagrama UDP para un puerto que no está en uso, no sabe qué hacer con él. Para notificar al emisor del problema, la capa de red del servidor genera un mensaje ICMP de tipo "Destino Inalcanzable" con el código específico "Puerto Inalcanzable" y lo envía a la IP de origen del datagrama.

</details>

---

**17. Durante el `Three-Way Handshake` de TCP, el cliente envía un segmento SYN con `Seq=X`. ¿Qué valores tendrán los campos `Seq` y `Ack` en la respuesta del servidor (el segundo paso del handshake)?**

- a) `Seq=X+1`, `Ack=Y+1` (donde Y es el ISN del servidor)
- b) `Seq=Y`, `Ack=X`
- c) `Seq=X+1`, `Ack=Y`
- d) `Seq=Y`, `Ack=X+1` (donde Y es el ISN del servidor)

<details>
<summary>Ver Respuesta</summary>

**Respuesta correcta: d)**

**Explicación:** El handshake funciona así:
1. Cliente -> Servidor: `SYN`, `Seq=X`.
2. Servidor -> Cliente: `SYN, ACK`, `Seq=Y` (el ISN del servidor), `Ack=X+1` (confirma la recepción del SYN del cliente, que consume un número de secuencia).
3. Cliente -> Servidor: `ACK`, `Seq=X+1`, `Ack=Y+1` (confirma la recepción del SYN del servidor).
Por tanto, la respuesta del servidor lleva su propio número de secuencia (`Y`) y el `ACK` del número de secuencia del cliente más uno.

</details>

---

**18. En una conexión TCP establecida, un emisor envía un segmento con 1000 bytes de datos y `Seq=5000`. El receptor recibe el segmento correctamente. Si el siguiente segmento que envía el receptor es puramente de confirmación (sin datos), ¿cuál será su número de `ACK`?**

- a) 5001
- b) 6000
- c) 5999
- d) 5000

<details>
<summary>Ver Respuesta</summary>

**Respuesta correcta: b)**

**Explicación:** El número de ACK (`Acknowledgment number`) en TCP indica el *siguiente* número de secuencia que el receptor espera recibir. Si ha recibido un segmento que comienza en `Seq=5000` y contiene 1000 bytes de datos, los bytes recibidos son del 5000 al 5999 inclusive. Por lo tanto, el siguiente byte que espera es el 6000. El `ACK` que enviará será `6000`.

</details>

---

**19. ¿Qué problema resuelve principalmente el Algoritmo de Nagle en TCP?**

- a) La pérdida de paquetes en redes congestionadas.
- b) El problema de los "tinygrams" (muchos paquetes pequeños con poca carga útil) en aplicaciones interactivas.
- c) La asignación de números de secuencia iniciales para evitar colisiones con conexiones antiguas.
- d) El reordenamiento de paquetes que llegan fuera de secuencia al receptor.

<details>
<summary>Ver Respuesta</summary>

**Respuesta correcta: b)**

**Explicación:** En aplicaciones interactivas (como telnet/ssh), cada pulsación de tecla puede generar un paquete. Enviar un paquete de 41 bytes (20 IP + 20 TCP + 1 dato) por cada byte es muy ineficiente. El algoritmo de Nagle intenta solucionar esto: si hay datos pequeños por enviar, espera un poco (hasta recibir un ACK por los datos ya enviados o hasta acumular suficientes datos para llenar un segmento) antes de enviar el paquete. Esto agrupa varios bytes pequeños en un solo segmento más grande, reduciendo la sobrecarga.

</details>

---

**20. ¿Qué indica el evento "triple duplicate ACK" en el control de congestión de TCP?**

- a) Que la red está severamente congestionada y es necesario un timeout.
- b) Que probablemente se ha perdido un segmento, pero los siguientes están llegando correctamente, y dispara el mecanismo de "Fast Retransmit".
- c) Que el tamaño de la ventana del receptor es cero y el emisor debe detenerse.
- d) Que la conexión debe terminarse inmediatamente debido a un error irrecuperable.

<details>
<summary>Ver Respuesta</summary>

**Respuesta correcta: b)**

**Explicación:** Si el emisor envía los segmentos 1, 2, 3, 4, 5 y el 3 se pierde, el receptor recibirá 1, 2, 4, 5. Por el 1 y 2, enviará ACKs normales. Al recibir el 4, se da cuenta de que falta el 3, y vuelve a enviar el ACK del último byte contiguo que recibió (el ACK para el segmento 2). Hará lo mismo al recibir el 5. El emisor recibirá un ACK normal y luego tres ACKs duplicados. Esto es una señal fuerte de que el segmento 3 se perdió. En lugar de esperar el timeout (RTO), TCP activa "Fast Retransmit" y reenvía inmediatamente el segmento perdido, entrando en la fase "Fast Recovery" para ajustar la ventana de congestión de forma menos drástica que con un timeout.

</details>

---

**21. ¿Cuál de los siguientes protocolos de aplicación se basa en UDP por defecto, priorizando la velocidad sobre la fiabilidad?**

- a) HTTP
- b) SMTP
- c) DNS
- d) FTP

<details>
<summary>Ver Respuesta</summary>

**Respuesta correcta: c)**

**Explicación:** Las consultas de DNS suelen ser peticiones y respuestas pequeñas y transaccionales. Usar UDP es mucho más rápido que establecer una conexión TCP completa para una consulta simple. Si una respuesta DNS se pierde, el cliente simplemente puede reintentar la consulta. HTTP, SMTP y FTP requieren una transferencia de datos fiable y, por lo tanto, utilizan TCP.

</details>

---

**22. En el contexto de HTTP, ¿cuál es la diferencia clave entre una conexión persistente sin pipelining y una con pipelining?**

- a) Sin pipelining, se necesita una conexión TCP nueva por cada objeto. Con pipelining, se reutiliza la conexión.
- b) Sin pipelining, el cliente espera la respuesta a una petición antes de enviar la siguiente. Con pipelining, el cliente puede enviar múltiples peticiones sin esperar sus respuestas.
- c) El pipelining solo funciona en HTTP/2, mientras que las conexiones persistentes son de HTTP/1.1.
- d) El pipelining comprime las cabeceras HTTP, mientras que las conexiones persistentes no.

<details>
<summary>Ver Respuesta</summary>

**Respuesta correcta: b)**

**Explicación:** Ambas técnicas utilizan una conexión TCP persistente (la misma conexión para múltiples peticiones/respuestas). La diferencia está en cómo se envían las peticiones.
- **Sin pipelining:** Cliente pide `obj1`, espera respuesta de `obj1`, pide `obj2`, espera respuesta de `obj2`.
- **Con pipelining (HTTP/1.1):** Cliente pide `obj1`, `obj2`, `obj3` una detrás de otra, y luego recibe las respuestas en el mismo orden.
El pipelining mejora la latencia, pero fue problemático de implementar y fue sustituido por la multiplexación real en HTTP/2.

</details>

---

**23. ¿Qué problema fundamental de HTTP/1.1 soluciona la multiplexación de HTTP/2?**

- a) El problema del "Head-of-Line Blocking" a nivel de TCP.
- b) El problema del "Head-of-Line Blocking" a nivel de aplicación.
- c) La falta de cifrado en las comunicaciones.
- d) La necesidad de usar direcciones IP en lugar de nombres de dominio.

<details>
<summary>Ver Respuesta</summary>

**Respuesta correcta: b)**

**Explicación:** En HTTP/1.1 con pipelining, si pides un objeto grande (A) y luego uno pequeño (B), el servidor debe enviar la respuesta de A completa antes de poder enviar la de B. Si A tarda mucho, B queda bloqueado, aunque sea pequeño. Esto es el "Head-of-Line Blocking" a nivel de aplicación. HTTP/2 soluciona esto introduciendo "streams". Las respuestas para A y B se pueden intercalar en trozos sobre la misma conexión TCP, de modo que B no tiene que esperar a que A termine. (Ojo: el HOL blocking a nivel de TCP sigue existiendo y es lo que motiva a HTTP/3 a usar QUIC sobre UDP).

</details>

---

**24. ¿Cuál es el rol de un registro MX en el sistema DNS?**

- a) Mapear un nombre de dominio a una dirección IPv6.
- b) Especificar el servidor de correo (`Mail eXchanger`) responsable de recibir emails para un dominio.
- c) Crear un alias o apodo para un nombre de dominio (registro canónico).
- d) Almacenar texto arbitrario para verificaciones de dominio u otros propósitos (SPF, DKIM).

<details>
<summary>Ver Respuesta</summary>

**Respuesta correcta: b)**

**Explicación:** Cuando un servidor de correo necesita enviar un email a `usuario@ejemplo.com`, no intenta conectar directamente a `ejemplo.com`. Primero, realiza una consulta DNS de tipo `MX` para el dominio `ejemplo.com`. La respuesta DNS le dará una lista de nombres de host (p.ej., `mail1.ejemplo.com`, `mail2.ejemplo.com`) que son los servidores de correo autorizados para ese dominio, junto con una prioridad. El servidor de envío intentará entonces entregar el email a esos hosts.

</details>

---

**25. Un cliente DNS realiza una consulta recursiva a su servidor DNS local para `www.ejemplo.com`. El servidor local no tiene la respuesta en caché. ¿Cuál de los siguientes describe mejor el proceso que seguirá el servidor DNS local?**

- a) Devuelve una lista de los servidores raíz al cliente para que este continúe la consulta.
- b) Contacta a un servidor raíz, luego a un servidor TLD para `.com`, y finalmente a un servidor autoritativo para `ejemplo.com`, y devuelve la respuesta final al cliente.
- c) Envía la petición a todos sus servidores DNS vecinos con la esperanza de que alguno conozca la respuesta.
- d) Devuelve un error al cliente indicando que la respuesta no está en caché.

<details>
<summary>Ver Respuesta</summary>

**Respuesta correcta: b)**

**Explicación:** En una consulta recursiva, el cliente delega todo el trabajo en su servidor DNS configurado. Este servidor es responsable de resolver el nombre por completo. Para ello, realiza consultas iterativas por su cuenta: pregunta al servidor raíz (que le dice quién gestiona `.com`), luego al servidor TLD de `.com` (que le dice quién gestiona `ejemplo.com`), y finalmente al servidor autoritativo de `ejemplo.com`, que le da la IP de `www.ejemplo.com`. Una vez tiene la respuesta final, la almacena en caché y la devuelve al cliente original.

</details>

---

**26. ¿Cuál es la diferencia principal entre un hipervisor de Tipo 1 (bare-metal) y uno de Tipo 2 (hosted)?**

- a) El Tipo 1 se instala sobre un sistema operativo anfitrión, mientras que el Tipo 2 se instala directamente sobre el hardware.
- b) El Tipo 1 es siempre de código abierto, mientras que el Tipo 2 es siempre propietario.
- c) El Tipo 1 se instala directamente sobre el hardware del servidor, mientras que el Tipo 2 se ejecuta como una aplicación dentro de un sistema operativo anfitrión.
- d) El Tipo 1 solo puede ejecutar un sistema operativo invitado, mientras que el Tipo 2 puede ejecutar múltiples.

<details>
<summary>Ver Respuesta</summary>

**Respuesta correcta: c)**

**Explicación:** Esta es la distinción fundamental.
- **Hipervisor Tipo 1 (ej. VMware ESXi, Xen, Hyper-V):** Actúa como un "mini-SO" cuyo único propósito es gestionar máquinas virtuales. Se instala en el hardware "desnudo" (bare-metal) y ofrece mayor rendimiento y seguridad, siendo el estándar en centros de datos.
- **Hipervisor Tipo 2 (ej. VMware Workstation, VirtualBox, Parallels):** Es una aplicación que se instala en un SO convencional (como Windows, macOS o Linux). Es más fácil de usar para desarrollo o pruebas en un PC personal, pero tiene una capa extra de sobrecarga (el SO anfitrión) que reduce su rendimiento.

</details>

---

**27. En el contexto de la virtualización de plataforma, ¿qué es la paravirtualización?**

- a) Una técnica donde el sistema operativo invitado no es modificado y cree que se ejecuta sobre hardware real.
- b) La emulación completa de una arquitectura de hardware diferente (por ejemplo, emular ARM en una CPU x86).
- c) Una técnica donde el sistema operativo invitado se modifica para ser consciente de que está siendo virtualizado y colabora con el hipervisor a través de `hypercalls`.
- d) La virtualización de una única aplicación en lugar de un sistema operativo completo.

<details>
<summary>Ver Respuesta</summary>

**Respuesta correcta: c)**

**Explicación:** La paravirtualización (PV) busca mejorar el rendimiento eliminando la sobrecarga de traducir instrucciones privilegiadas. Para ello, el kernel del sistema operativo invitado es modificado para que, en lugar de ejecutar instrucciones que causarían un fallo en un entorno virtual, realice llamadas directas al hipervisor (hypercalls) para solicitar esas operaciones. Esto requiere un SO invitado "consciente" de la virtualización. La virtualización completa (Full Virtualization) no requiere modificar el SO, pero es más lenta si no cuenta con asistencia por hardware.

</details>

---

**28. ¿Qué ventaja clave ofrece la virtualización asistida por hardware (Intel VT-x / AMD-V)?**

- a) Permite que el hipervisor se ejecute en el anillo 3 (modo usuario) para mayor seguridad.
- b) Elimina la necesidad de un hipervisor, permitiendo que las VMs se comuniquen directamente.
- c) Introduce un nuevo modo de ejecución en la CPU que permite al hipervisor interceptar y gestionar instrucciones privilegiadas de forma eficiente sin necesidad de traducción binaria o paravirtualización.
- d) Comprime la memoria de las máquinas virtuales para que ocupen menos RAM.

<details>
<summary>Ver Respuesta</summary>

**Respuesta correcta: c)**

**Explicación:** Las tecnologías como VT-x y AMD-V añaden extensiones al conjunto de instrucciones de la CPU. Crean un nuevo modo de ejecución (a veces llamado "ring -1") que permite al hipervisor ejecutarse con el máximo privilegio, mientras que el SO invitado se ejecuta en anillo 0. Cuando el SO invitado intenta ejecutar una instrucción privilegiada, la CPU automáticamente y de forma transparente cede el control al hipervisor, en lugar de generar un fallo. El hipervisor gestiona la operación y devuelve el control. Esto permite que sistemas operativos no modificados se ejecuten de forma segura y con un rendimiento muy cercano al nativo.

</details>

---

**29. ¿Qué es un "contenedor" (como Docker) y en qué se diferencia fundamentalmente de una máquina virtual tradicional?**

- a) Un contenedor es una forma más pesada de virtualización que incluye un sistema operativo completo.
- b) Un contenedor virtualiza el hardware, mientras que una VM virtualiza el sistema operativo.
- c) Un contenedor es una instancia de una VM optimizada para ejecutarse en la nube.
- d) Un contenedor comparte el kernel del sistema operativo del host y solo empaqueta una aplicación y sus dependencias, siendo mucho más ligero y rápido que una VM.

<details>
<summary>Ver Respuesta</summary>

**Respuesta correcta: d)**

**Explicación:** La diferencia clave es la capa de abstracción.
- **Máquina Virtual:** Virtualiza el hardware. Cada VM tiene su propio sistema operativo completo (kernel, librerías, etc.), lo que la hace pesada (GBs de tamaño) y lenta para arrancar.
- **Contenedor:** Virtualiza el sistema operativo. Todos los contenedores que se ejecutan en un host comparten el mismo kernel del SO del host. El contenedor solo incluye la aplicación y sus librerías y binarios específicos. Esto los hace extremadamente ligeros (MBs de tamaño) y rápidos (arrancan en milisegundos).

</details>

---

**30. En los modelos de servicio en la nube, ¿cuál es la diferencia principal de responsabilidad entre IaaS (Infrastructure as a Service) y PaaS (Platform as a Service)?**

- a) En IaaS el cliente gestiona la infraestructura física, mientras que en PaaS la gestiona el proveedor.
- b) En IaaS el cliente gestiona el sistema operativo y el middleware, mientras que en PaaS esa gestión recae en el proveedor.
- c) IaaS se usa para desplegar aplicaciones web, mientras que PaaS se usa para almacenamiento de datos.
- d) En IaaS el pago es por uso, mientras que en PaaS es una suscripción fija mensual.

<details>
<summary>Ver Respuesta</summary>

**Respuesta correcta: b)**

**Explicación:** La línea divisoria de responsabilidad se mueve hacia arriba en el stack:
- **IaaS (ej. AWS EC2, Google Compute Engine):** El proveedor te da la infraestructura básica virtualizada (servidores, red, almacenamiento). Tú, como cliente, eres responsable de instalar, configurar y gestionar todo lo que va encima: el sistema operativo, el middleware (servidores de bases de datos, de aplicaciones) y tu aplicación.
- **PaaS (ej. Heroku, Google App Engine):** El proveedor gestiona la infraestructura Y la plataforma (SO, middleware, runtime). Tú solo te preocupas de desplegar y gestionar tu código y tus datos. No te preocupas de actualizar el SO ni de parchear la base de datos.

</details>

---

**31. ¿Qué característica del Cloud Computing describe la capacidad de un sistema para aumentar o disminuir automáticamente los recursos asignados a una aplicación en respuesta a la demanda?**

- a) Multitenancy (Alquiler múltiple)
- b) Resiliencia
- c) Elasticidad
- d) Acceso ubicuo

<details>
<summary>Ver Respuesta</summary>

**Respuesta correcta: c)**

**Explicación:** La elasticidad es una de las propuestas de valor más importantes de la nube. Se refiere a la capacidad de provisionar y desprovisionar recursos de forma dinámica. Si tu web tiene un pico de tráfico, un sistema elástico puede añadir automáticamente más servidores web (escalado horizontal o "scale-out"). Cuando el tráfico baja, esos servidores se eliminan para no pagar por ellos. Esto permite adaptar los recursos (y los costes) a la demanda real.

</details>

---

**32. ¿Qué modelo de despliegue de nube implica una infraestructura operada exclusivamente para una única organización, ya sea gestionada internamente o por un tercero, y que puede existir on-premise o off-premise?**

- a) Nube Pública
- b) Nube Híbrida
- c) Nube Comunitaria
- d) Nube Privada

<details>
<summary>Ver Respuesta</summary>

**Respuesta correcta: d)**

**Explicación:** Una nube privada es un entorno de cloud computing dedicado a una sola organización. Ofrece las ventajas de la nube (autoservicio, automatización) pero con mayor control, seguridad y privacidad, ya que los recursos no se comparten con otros "inquilinos". Esto contrasta con la nube pública, donde la infraestructura es compartida por múltiples clientes.

</details>

---

**33. Un cliente TCP está en el estado `FIN_WAIT_1`. ¿Qué ha ocurrido justo antes de entrar en este estado?**

- a) El cliente ha recibido un segmento FIN del servidor.
- b) El cliente ha enviado un segmento FIN al servidor para iniciar el cierre de la conexión.
- c) La aplicación del cliente ha cerrado el socket, pero TCP aún no ha hecho nada.
- d) El cliente ha recibido un ACK del FIN que envió previamente.

<details>
<summary>Ver Respuesta</summary>

**Respuesta correcta: b)**

**Explicación:** El estado `FIN_WAIT_1` es el primer paso en el cierre activo de una conexión. La secuencia es:
1. La aplicación en el cliente decide cerrar la conexión.
2. El TCP del cliente envía un segmento `FIN` al servidor.
3. Inmediatamente después de enviar el `FIN`, el cliente pasa al estado `FIN_WAIT_1`, donde espera un `ACK` de su `FIN` por parte del servidor.

</details>

---

**34. ¿Qué es un ataque de "ARP Spoofing" o "ARP Poisoning"?**

- a) Un ataque que inunda la red con peticiones ARP para agotar los recursos de los switches.
- b) Un ataque en el que un atacante envía respuestas ARP falsificadas para asociar su propia dirección MAC con la dirección IP de otro host (como el gateway), interceptando así el tráfico.
- c) Un ataque que modifica el TTL de los paquetes ARP para crear bucles de enrutamiento.
- d) Un ataque que utiliza el protocolo ARP para descubrir hosts vulnerables en una red.

<details>
<summary>Ver Respuesta</summary>

**Respuesta correcta: b)**

**Explicación:** ARP no tiene mecanismos de autenticación. Un atacante en la misma LAN puede enviar respuestas ARP no solicitadas a un host víctima, diciéndole "Yo soy el router, mi MAC es `[MAC_DEL_ATACANTE]`". La víctima actualiza su caché ARP con esta información falsa. A partir de ese momento, todo el tráfico que la víctima intente enviar al router (y por tanto, a Internet) pasará primero por la máquina del atacante, permitiéndole leer o modificar el tráfico (ataque "Man-in-the-Middle").

</details>

---

**35. En una red que utiliza `CSMA/CD`, ¿por qué existe un tamaño mínimo de trama (64 bytes para Ethernet)?**

- a) Para asegurar que la cabecera IP quepa completamente en la trama.
- b) Para que el tiempo de transmisión de la trama sea suficientemente largo como para que una colisión pueda ser detectada por el emisor en el peor de los casos.
- c) Es un estándar arbitrario para simplificar el diseño de los switches.
- d) Para garantizar que el campo de datos no esté vacío.

<details>
<summary>Ver Respuesta</summary>

**Respuesta correcta: b)**

**Explicación:** Este es un concepto clave de CSMA/CD. En el peor caso, una colisión ocurre en el extremo más alejado de la red justo antes de que la señal llegue allí. La "señal de colisión" debe tener tiempo de viajar de vuelta al emisor original. El emisor debe seguir transmitiendo hasta que esa señal de colisión pueda, teóricamente, llegarle. Si terminara de transmitir una trama muy corta antes de que la señal de colisión le llegue, nunca se enteraría de la colisión. El tamaño mínimo de trama asegura que `Tiempo de Transmisión >= 2 * Tiempo de Propagación máximo` (el tiempo de ida y vuelta en la red).

</details>

---

**36. ¿Cuál de las siguientes afirmaciones sobre los puertos `well-known` (0-1023) es la MÁS precisa?**

- a) Solo los protocolos TCP pueden usarlos; UDP está restringido a puertos superiores.
- b) En la mayoría de los sistemas operativos, solo los procesos con privilegios de administrador (root) pueden abrir un servidor en estos puertos.
- c) Cualquier aplicación puede usar libremente un puerto `well-known` siempre que no esté ya en uso.
- d) Estos puertos se asignan dinámicamente a las aplicaciones cliente para cada conexión saliente.

<details>
<summary>Ver Respuesta</summary>

**Respuesta correcta: b)**

**Explicación:** Los puertos del 0 al 1023 están estandarizados por la IANA para servicios de sistema críticos (HTTP en el 80, FTP en el 21, etc.). Para prevenir que programas de usuario no autorizados suplanten estos servicios estándar, los sistemas operativos como Unix/Linux y Windows requieren privilegios elevados para que una aplicación pueda vincularse a uno de estos puertos y aceptar conexiones. Los puertos para clientes (efímeros) se eligen de un rango mucho más alto.

</details>

---

**37. Un servidor TCP tiene una `congestion window` (cwnd) de 8 MSS y un `slow start threshold` (ssthresh) de 12 MSS. Recibe ACKs para los 8 segmentos enviados sin pérdidas. ¿Cuál será el nuevo valor de la `cwnd` para la siguiente ronda de envío?**

- a) 9 MSS
- b) 16 MSS
- c) 12 MSS
- d) 8 MSS

<details>
<summary>Ver Respuesta</summary>

**Respuesta correcta: b)**

**Explicación:** La `cwnd` es menor que `ssthresh`, por lo que la conexión se encuentra en la fase de **Slow Start**. En esta fase, el crecimiento de la `cwnd` es exponencial. Por cada ACK recibido, la `cwnd` se incrementa en 1 MSS. Como se enviaron 8 segmentos y se recibieron 8 ACKs (asumiendo uno por segmento), la `cwnd` se incrementará en 8 MSS. El nuevo valor será `8 + 8 = 16 MSS`. Sin embargo, como el `ssthresh` es 12, la `cwnd` crecerá hasta 12 en modo Slow Start y luego cambiará a Congestion Avoidance. En la práctica, muchos stacks TCP doblan la `cwnd` por cada RTT, por lo que `8 -> 16` es la respuesta conceptual. Dado que 16 supera el umbral, el crecimiento se detendrá en 12 y cambiará de fase. La pregunta es sutil: la fase actual es Slow Start y la regla es duplicar por RTT. La cwnd se convertiría en 16, pero al superar el ssthresh, en la siguiente ronda ya estaría en modo Congestion Avoidance y su crecimiento sería lineal. No obstante, el crecimiento *dentro* de la fase de Slow Start es exponencial. La opción más correcta que refleja la regla de Slow Start es 16.

*Nota de profesor "troll":* Una pregunta aún más difícil especificaría el momento exacto. Pero la regla de Slow Start es duplicar la ventana cada RTT. El ssthresh limita cuándo se cambia de fase. Al recibir los 8 ACKs, la ventana se duplica a 16. El *próximo* envío ya estaría sujeto a las reglas de Congestion Avoidance, pero el cálculo inmediato de la nueva cwnd la llevaría a 16 antes de ser capada o cambiar de modo.

</details>

---

**38. ¿Qué es el "Split Horizon" en un protocolo de enrutamiento por vector-distancia como RIP?**

- a) Una técnica que divide la red en múltiples áreas para reducir el tráfico de enrutamiento.
- b) Una regla que impide a un router anunciar una ruta de vuelta por la misma interfaz de la que la aprendió.
- c) Un mecanismo para agregar múltiples rutas en una sola entrada de la tabla de enrutamiento.
- d) El tiempo que una ruta permanece en la tabla antes de ser considerada inválida si no se refresca.

<details>
<summary>Ver Respuesta</summary>

**Respuesta correcta: b)**

**Explicación:** El "Split Horizon" es una medida para prevenir bucles de enrutamiento en los protocolos de vector-distancia. El problema es el "count-to-infinity". Si el Router A aprende una ruta hacia la red X a través del Router B, A no debe anunciar de vuelta a B que puede llegar a X. Si lo hiciera, y el enlace original a X desde B cayera, B podría pensar erróneamente que puede llegar a X a través de A, creando un bucle donde A y B se envían paquetes para X el uno al otro. El "Split Horizon" evita esto al no anunciar la ruta de vuelta.

</details>

---

**39. Estás analizando una captura de Wireshark de una conexión TCP. Observas un segmento con los flags `FIN` y `PSH` activados. ¿Qué significa esta combinación?**

- a) Es un segmento inválido; FIN y PSH no pueden ir juntos.
- b) Es el último segmento de datos enviado por el emisor. El emisor está empujando los datos restantes a la aplicación receptora y, al mismo tiempo, notificando que no enviará más datos.
- c) Es una petición para resetear la conexión mientras se envían datos urgentes.
- d) Es un segmento del handshake inicial que también lleva datos de la aplicación.

<details>
<summary>Ver Respuesta</summary>

**Respuesta correcta: b)**

**Explicación:** Los flags de TCP pueden combinarse.
- `FIN` indica que el emisor ha terminado de enviar datos. Es parte del proceso de cierre de la conexión.
- `PSH` (Push) es una indicación para la capa TCP receptora de que debe entregar los datos que ha recibido hasta ahora a la aplicación sin esperar a que su buffer se llene.
La combinación `FIN, PSH` es perfectamente válida y común. Significa que este es el último trozo de datos que el emisor va a enviar, y le está diciendo al receptor: "Aquí tienes mis últimos datos, entrégaselos ya a la aplicación, que por mi parte he terminado".

</details>

---

**40. ¿Cuál es el propósito del mecanismo `SO_REUSEADDR` que se puede configurar en un socket TCP antes de hacer `bind()`?**

- a) Permite que múltiples aplicaciones escuchen en el mismo puerto simultáneamente, creando un balanceo de carga.
- b) Permite a un servidor reiniciarse y volver a vincularse a un puerto que acaba de cerrar y que podría estar temporalmente en estado `TIME_WAIT`.
- c) Reutiliza el último número de puerto efímero que usó un cliente para una nueva conexión.
- d) Obliga a TCP a reutilizar los números de secuencia de una conexión anterior para ahorrar memoria.

<details>
<summary>Ver Respuesta</summary>

**Respuesta correcta: b)**

**Explicación:** Cuando una conexión TCP se cierra, el socket en el lado que cerró la conexión puede permanecer en estado `TIME_WAIT` durante un par de minutos para asegurar que cualquier paquete retardado de la antigua conexión no sea aceptado por una nueva conexión con el mismo par (IP:puerto). Durante este tiempo, el sistema operativo no permitiría a otro proceso (o al mismo si se reinicia) vincularse a ese mismo puerto. `SO_REUSEADDR` relaja esta regla, permitiendo que un nuevo proceso de escucha se vincule al puerto incluso si hay un socket de una conexión anterior en estado `TIME_WAIT` asociado a él. Esto es crucial para que los servidores puedan reiniciarse rápidamente sin tener que esperar a que el estado `TIME_WAIT` expire.

</details>
