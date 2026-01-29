# Test de Autoevaluación - Tema 6: Enrutamiento e ICMP

Este test contiene preguntas de opción múltiple diseñadas para evaluar la comprensión de los algoritmos de enrutamiento, las tablas de enrutamiento, CIDR y el protocolo ICMP.

---

### Pregunta 1

Un router tiene las siguientes tres entradas en su tabla de enrutamiento:
1. `192.168.1.0/24` vía `10.0.0.1`
2. `192.168.0.0/16` vía `10.0.0.2`
3. `0.0.0.0/0` (ruta por defecto) vía `10.0.0.3`

Si llega un paquete con la dirección IP de destino `192.168.1.10`, ¿a qué siguiente salto lo enviará el router?

*   a) `10.0.0.1`
*   b) `10.0.0.2`
*   c) `10.0.0.3`
*   d) El paquete será descartado.

<details>
  <summary>Ver Respuesta</summary>
  
  **Respuesta correcta: a)** 
  
  **Justificación:** El principio de enrutamiento que se aplica es el de "longest prefix match" (coincidencia del prefijo más largo). La IP de destino `192.168.1.10` coincide con las tres rutas:
- Coincide con `192.168.1.0/24` (los primeros 24 bits son iguales).
- Coincide con `192.168.0.0/16` (los primeros 16 bits son iguales).
- Coincide con `0.0.0.0/0` (cualquier IP coincide con la ruta por defecto).

El router siempre elegirá la ruta con la máscara de subred más específica (el prefijo más largo), que en este caso es `/24`. Por lo tanto, el paquete se enviará al gateway `10.0.0.1`.
</details>

---

### Pregunta 2

Estás utilizando la herramienta `traceroute` (o `tracert` en Windows) para diagnosticar un problema de red hacia el servidor `8.8.8.8`. ¿Qué técnica utiliza esta herramienta para descubrir los routers intermedios en la ruta?

*   a) Envía peticiones ARP a cada router en el camino.
*   b) Usa el campo de "Record Route" en la cabecera IP para que cada router apunte su dirección.
*   c) Envía una serie de paquetes (UDP o ICMP) con un valor de TTL (Time To Live) que se va incrementando, provocando que cada router sucesivo en la ruta descarte el paquete y devuelva un error ICMP "Time Exceeded".
*   d) Se conecta a un servidor central que tiene un mapa completo de todas las rutas de Internet.

<details>
  <summary>Ver Respuesta</summary>
  
  **Respuesta correcta: c)** 
  
  **Justificación:** `traceroute` funciona de manera ingeniosa. Primero, envía un paquete con TTL=1. El primer router en la ruta decrementa el TTL a 0, descarta el paquete y envía un ICMP "Time Exceeded" al origen. El origen registra la IP de ese router. Luego, envía un paquete con TTL=2. El primer router lo pasa, pero el segundo lo descarta y responde. Este proceso se repite, incrementando el TTL en cada paso, para trazar la ruta salto a salto hasta llegar al destino.
</details>

---

### Pregunta 3

Un ISP (Proveedor de Servicios de Internet) posee el bloque de direcciones `200.20.0.0/20`. Un cliente corporativo le solicita un bloque de 1000 direcciones IP. ¿Cuál de las siguientes asignaciones mediante CIDR (Classless Inter-Domain Routing) satisface la petición del cliente de forma más eficiente?

*   a) `200.20.0.0/24` (254 hosts)
*   b) `200.20.0.0/23` (510 hosts)
*   c) `200.20.0.0/22` (1022 hosts)
*   d) `200.20.0.0/21` (2046 hosts)

<details>
  <summary>Ver Respuesta</summary>
  
  **Respuesta correcta: c)** 
  
  **Justificación:** Se busca el prefijo de red que ofrezca un número de direcciones de host disponibles (2^(32-prefijo) - 2) que sea igual o inmediatamente superior a 1000.
- /24 ofrece 2^8 - 2 = 254 hosts (insuficiente).
- /23 ofrece 2^9 - 2 = 510 hosts (insuficiente).
- /22 ofrece 2^10 - 2 = 1022 hosts (suficiente y más eficiente).
- /21 ofrece 2^11 - 2 = 2046 hosts (suficiente, pero desperdicia más direcciones que /22).
La notación /22 es la que mejor se ajusta, proveyendo la cantidad necesaria con el menor desperdicio de IPs.
</details>

---

### Pregunta 4

¿Cuál es la principal diferencia entre el enrutamiento estático y el enrutamiento dinámico?

*   a) El enrutamiento estático es más seguro porque las rutas nunca cambian.
*   b) En enrutamiento estático, las rutas son configuradas manualmente por un administrador, mientras que en enrutamiento dinámico, los routers intercambian información entre sí para construir y actualizar sus tablas de enrutamiento automáticamente.
*   c) El enrutamiento dinámico solo funciona con IPv6, mientras que el estático solo funciona con IPv4.
*   d) El enrutamiento estático siempre elige la ruta más corta, mientras que el dinámico puede elegir rutas más largas pero más rápidas.

<details>
  <summary>Ver Respuesta</summary>
  
  **Respuesta correcta: b)**
  
  **Justificación:** La diferencia fundamental radica en cómo se pueblan y mantienen las tablas de enrutamiento. En el enrutamiento estático, un administrador debe definir explícitamente cada ruta. Esto es simple para redes pequeñas y estables, pero no escala. En el enrutamiento dinámico, los routers ejecutan protocolos (como OSPF o BGP) que les permiten descubrir vecinos, anunciar las redes que conocen y calcular las mejores rutas de forma automática, adaptándose a los cambios en la topología de la red.
</details>

---

### Pregunta 5

Recibes un mensaje ICMP de tipo "Destination Unreachable" con el código "Port Unreachable". ¿Cuál es el escenario más probable que ha causado este error?

*   a) El router no ha encontrado una ruta para llegar a la dirección IP de destino.
*   b) Un paquete fue enviado a una dirección IP correcta, pero el dispositivo de destino no tiene ningún servicio o aplicación escuchando en el puerto UDP de destino.
*   c) El TTL del paquete expiró antes de llegar a su destino.
*   d) La dirección IP de destino no existe en Internet.

<details>
  <summary>Ver Respuesta</summary>
  
  **Respuesta correcta: b)**
  
  **Justificación:** ICMP es el protocolo de diagnóstico de IP. El mensaje "Destination Unreachable" tiene varios códigos para especificar la causa. El código "Port Unreachable" se genera cuando un datagrama (típicamente UDP, ya que TCP gestionaría esto con un RST) llega correctamente al host de destino (la IP es correcta), pero el sistema operativo no encuentra ningún socket de aplicación asociado al número de puerto de destino especificado en el datagrama.
</details>
