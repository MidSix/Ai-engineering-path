# Test de Autoevaluación - Tema 7: UDP y TCP

Este test contiene preguntas de opción múltiple diseñadas para evaluar la comprensión de los protocolos de la capa de transporte, UDP y TCP, sus diferencias, y los mecanismos de establecimiento y finalización de conexión.

---

### Pregunta 1

Una aplicación de streaming de vídeo en directo necesita enviar datos a sus espectadores. La prioridad es la velocidad y la mínima latencia. Perder un fotograma de vez en cuando es aceptable si eso evita que el vídeo se detenga (buffering). ¿Qué protocolo de transporte es más adecuado para esta tarea y por qué?

*   a) TCP, porque garantiza que todos los fotogramas lleguen y en el orden correcto.
*   b) UDP, porque no tiene sobrecarga de establecimiento de conexión ni retransmisiones, lo que minimiza el retardo.
*   c) TCP, porque su control de congestión asegura que no se sature la red del espectador.
*   d) UDP, porque el campo checksum es opcional, ahorrando tiempo de procesamiento.

<details>
  <summary>Ver Respuesta</summary>
  
  **Respuesta correcta: b)** 
  
  **Justificación:** UDP (User Datagram Protocol) es un protocolo no orientado a conexión y no fiable. No realiza handshake inicial, no retransmite paquetes perdidos y no garantiza el orden de llegada. Esta simplicidad se traduce en una latencia mucho menor, que es el factor crítico para aplicaciones en tiempo real como el vídeo en directo o las llamadas de voz, donde la inmediatez es más importante que la integridad total de los datos.
</details>

---

### Pregunta 2

Durante el establecimiento de una conexión TCP (el "Three-Way Handshake"), un cliente envía un segmento con el flag SYN activado y un número de secuencia inicial (ISN) de, por ejemplo, 5000. ¿Qué se espera que el servidor responda si está listo para aceptar la conexión?

*   a) Un segmento con el flag ACK activado y el número de ACK en 5000.
*   b) Un segmento con los flags SYN y ACK activados, su propio ISN (ej. 8000) y el número de ACK en 5001.
*   c) Un segmento con el flag FIN activado para indicarle al cliente que empiece a enviar datos.
*   d) Un segmento con los flags SYN y ACK activados, y el número de ACK en 5000.

<details>
  <summary>Ver Respuesta</summary>
  
  **Respuesta correcta: b)** 
  
  **Justificación:** En el segundo paso del handshake, el servidor debe hacer dos cosas: 1) Confirmar la recepción del SYN del cliente, para lo cual envía un ACK con el número de secuencia del cliente más uno (5000 + 1 = 5001). 2) Iniciar la conexión en su propia dirección, para lo cual envía su propio segmento SYN con su propio número de secuencia inicial (ISN). Ambos flags, SYN y ACK, viajan juntos en el segundo segmento del handshake.
</details>

---

### Pregunta 3

Un cliente TCP envía 1000 bytes de datos a un servidor. El último byte enviado tenía el número de secuencia 2500. Si todos los datos se reciben correctamente, ¿qué número de ACK enviará el servidor en su respuesta?

*   a) 2500
*   b) 2501
*   c) 3500
*   d) 3501

<details>
  <summary>Ver Respuesta</summary>
  
  **Respuesta correcta: d)** 
  
  **Justificación:** El número de ACK en TCP indica el número de secuencia del *siguiente* byte que espera recibir. Si el cliente envió 1000 bytes y el último fue el 2500, significa que el primer byte de ese segmento fue el 1501 (porque 1501 + 999 = 2500). El servidor ha recibido correctamente hasta el byte 2500, por lo que en su ACK de confirmación solicitará el siguiente byte, que es el 2501.
</details>

---

### Pregunta 4

¿Cuál es la función principal del campo "Tamaño de ventana" (Window Size) en la cabecera TCP?

*   a) Limitar el tamaño máximo de un segmento TCP que se puede enviar.
*   b) Indicar el número de bytes que el receptor está dispuesto a aceptar en su buffer de entrada, implementando así el control de flujo.
*   c) Negociar la versión de TCP que se va a utilizar durante la conexión.
*   d) Especificar el tiempo que un segmento puede permanecer en la red antes de ser descartado.

<details>
  <summary>Ver Respuesta</summary>
  
  **Respuesta correcta: b)**
  
  **Justificación:** El control de flujo es un mecanismo para evitar que un emisor rápido desborde el buffer de un receptor más lento. El campo "Tamaño de ventana" es la forma en que el receptor comunica al emisor cuánto espacio libre le queda en su buffer. Un emisor TCP no puede tener más bytes en tránsito (no confirmados por un ACK) que el último valor de ventana anunciado por el receptor.
</details>

---

### Pregunta 5

El estado `TIME_WAIT` ocurre en el lado que inicia el cierre de una conexión TCP, después de recibir el último FIN del otro extremo y enviar el ACK final. ¿Cuál es el propósito principal de este estado de espera?

*   a) Dar tiempo al servidor para que libere los recursos de la conexión.
*   b) Esperar a que el administrador de la red apruebe el cierre de la conexión.
*   c) Asegurarse de que el ACK final llega al otro extremo (reenviándolo si es necesario) y permitir que cualquier paquete retrasado de la conexión original muera en la red, evitando que interfiera con conexiones futuras.
*   d) Mantener la conexión semi-abierta por si el cliente cambia de opinión y quiere enviar más datos.

<details>
  <summary>Ver Respuesta</summary>
  
  **Respuesta correcta: c)**
  
  **Justificación:** El estado `TIME_WAIT` (también conocido como 2MSL wait state) tiene dos funciones críticas de fiabilidad. Primero, si el ACK final se pierde, el otro extremo entrará en timeout y reenviará su FIN. El lado en `TIME_WAIT` puede entonces reenviar el ACK. Segundo, y más importante, garantiza que cualquier paquete duplicado o retrasado de la conexión actual no sea interpretado erróneamente como parte de una nueva conexión que pudiera reutilizar el mismo par de sockets (misma IP y puertos de origen y destino) poco después.
</details>
