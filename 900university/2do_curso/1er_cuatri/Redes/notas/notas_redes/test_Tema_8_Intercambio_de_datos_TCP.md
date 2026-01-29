# Test de Autoevaluación - Tema 8: Intercambio de Datos TCP

Este test contiene preguntas de opción múltiple diseñadas para evaluar la comprensión de los mecanismos de transferencia de datos fiable de TCP, incluyendo la ventana deslizante, el control de flujo y el control de congestión.

---

### Pregunta 1

Un emisor TCP utiliza un protocolo de ventana deslizante. La ventana tiene un tamaño de 4 segmentos. El emisor envía los segmentos 1, 2, 3 y 4. El receptor recibe los segmentos 1, 2 y 4, pero el segmento 3 se pierde. ¿Qué ACK (o ACKs) enviará el receptor al recibir estos segmentos?

*   a) Enviará ACK 2, ACK 3, y luego ACK 5.
*   b) Enviará ACK 2, luego repetirá ACK 2 al recibir el segmento 4, indicando que aún espera el 3.
*   c) No enviará ningún ACK hasta que no llegue el segmento 3.
*   d) Enviará un ACK selectivo (SACK) por el segmento 4 y un ACK normal por el 2.

<details>
  <summary>Ver Respuesta</summary>
  
  **Respuesta correcta: b)** 
  
  **Justificación:** Los ACKs de TCP son acumulativos. Al recibir el segmento 1, el receptor envía ACK 2 (esperando el 2). Al recibir el 2, envía ACK 3 (esperando el 3). Cuando recibe el segmento 4 fuera de orden, lo almacena en su buffer, pero no puede confirmar más allá del último byte contiguo que ha recibido. Por lo tanto, vuelve a enviar un ACK por el siguiente byte que le falta en secuencia, que sigue siendo el 3. Este ACK duplicado es una señal para el emisor de que algo puede haber llegado fuera de orden o haberse perdido.
</details>

---

### Pregunta 2

El algoritmo de Nagle se diseñó para solucionar el problema de los "tinygrams" en sesiones interactivas (como Telnet/SSH) sobre redes WAN. ¿Cómo funciona básicamente este algoritmo?

*   a) Combina muchos pequeños paquetes de datos en un solo segmento grande antes de enviarlo.
*   b) Permite que una conexión TCP tenga como máximo un único segmento pequeño sin confirmar en la red; los datos pequeños adicionales se almacenan en un buffer hasta que llega el ACK del segmento anterior.
*   c) Retarda el envío de todos los ACKs durante 200 ms para agruparlos con datos de respuesta.
*   d) Aumenta el tamaño de la cabecera TCP para que el ratio datos/cabecera sea más favorable.

<details>
  <summary>Ver Respuesta</summary>
  
  **Respuesta correcta: b)** 
  
  **Justificación:** El objetivo de Nagle es reducir el número de segmentos pequeños (con mucha sobrecarga de cabeceras) en la red. Su regla es simple: si hay datos nuevos para enviar, pero ya hay un segmento enviado que no ha sido confirmado (acusado de recibo por un ACK), el emisor debe retener esos nuevos datos en un buffer. Solo cuando llega el ACK del segmento pendiente, o cuando se ha acumulado suficiente data para llenar un segmento completo (MSS), se envían los datos del buffer.
</details>

---

### Pregunta 3

Un emisor TCP recibe tres ACKs duplicados para el mismo número de secuencia. ¿Qué infiere el emisor de esta situación y qué mecanismo se activa?

*   a) Infiere que la red está completamente caída y cierra la conexión.
*   b) Infiere que el receptor está pidiendo que se acelere la transmisión.
*   c) Infiere que el segmento que sigue al número de ACK se ha perdido y activa el mecanismo de "Fast Retransmit" (retransmisión rápida) sin esperar a que expire el temporizador de retransmisión (RTO).
*   d) Infiere que el receptor ha cambiado su tamaño de ventana y ajusta su tasa de envío.

<details>
  <summary>Ver Respuesta</summary>
  
  **Respuesta correcta: c)** 
  
  **Justificación:** Recibir tres ACKs duplicados es una señal fuerte para el emisor de que el receptor ha recibido varios segmentos fuera de orden. La causa más probable es que un segmento intermedio se ha perdido. En lugar de esperar a que salte el temporizador de timeout (que puede ser un proceso lento), el algoritmo de TCP activa la "retransmisión rápida" y reenvía inmediatamente el segmento perdido. Esto mejora significativamente el rendimiento al recuperarse de pérdidas de paquetes de forma más ágil.
</details>

---

### Pregunta 4

¿Cuál es la diferencia fundamental entre el control de flujo y el control de congestión en TCP?

*   a) No hay ninguna diferencia, son dos nombres para el mismo mecanismo.
*   b) El control de flujo evita que el emisor sature al receptor, mientras que el control de congestión evita que el emisor sature la red.
*   c) El control de flujo lo gestiona el emisor, mientras que el control de congestión lo gestiona el receptor.
*   d) El control de flujo se usa en UDP, mientras que el control de congestión se usa en TCP.

<details>
  <summary>Ver Respuesta</summary>
  
  **Respuesta correcta: b)**
  
  **Justificación:** Aunque ambos mecanismos limitan la tasa de envío de datos, su propósito es distinto. El **control de flujo** es un mecanismo local, punto a punto, que protege al *receptor* de ser desbordado. Se implementa con la ventana deslizante y el campo "Window Size" de la cabecera TCP. El **control de congestión**, en cambio, es un mecanismo que protege a la *red* en su conjunto, intentando evitar la saturación de los routers intermedios. Se implementa con algoritmos como Slow Start, Congestion Avoidance, Fast Retransmit y Fast Recovery.
</details>

---

### Pregunta 5

Un servidor y un cliente mantienen una conexión TCP abierta pero inactiva durante varias horas. De repente, el cliente se apaga de forma abrupta debido a un corte de energía. El servidor no sabe nada de esto. ¿Qué mecanismo puede utilizar el servidor para detectar que la conexión ya no es viable y liberar los recursos asociados a ella?

*   a) El servidor esperará indefinidamente a que el cliente vuelva a contactarle.
*   b) El servidor enviará periódicamente un `DHCP Discover` para ver si el cliente responde.
*   c) El servidor puede utilizar el temporizador de persistencia para sondear al cliente.
*   d) El servidor puede utilizar el temporizador `keepalive`, que, tras un largo período de inactividad (ej. 2 horas), envía sondas periódicas para comprobar si el otro extremo sigue activo.

<details>
  <summary>Ver Respuesta</summary>
  
  **Respuesta correcta: d)**
  
  **Justificación:** El mecanismo de TCP keepalive está diseñado precisamente para este escenario (conexiones "medio abiertas"). Aunque no forma parte del estándar original de TCP, es una extensión muy común. Cuando una conexión ha estado inactiva durante un tiempo configurable (típicamente 2 horas), el servidor empieza a enviar segmentos de sondeo (keepalive probes). Si no recibe respuesta tras un número determinado de intentos, asume que el cliente ha desaparecido y cierra su extremo de la conexión, liberando memoria y otros recursos.
</details>
