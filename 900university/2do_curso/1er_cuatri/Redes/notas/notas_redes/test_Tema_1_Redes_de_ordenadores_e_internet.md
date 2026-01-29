# Test de Autoevaluación - Tema 1: Redes de Ordenadores e Internet

Este test contiene preguntas de opción múltiple diseñadas para evaluar la comprensión de los conceptos fundamentales del Tema 1, siguiendo un estilo práctico y basado en escenarios.

---

### Pregunta 1

Una startup está desarrollando una aplicación de videoconferencia en tiempo real. La prioridad es minimizar el retardo (latencia) para que la conversación sea fluida. La pérdida ocasional de un pequeño paquete de datos es aceptable, ya que es preferible a detener la imagen para esperar una retransmisión. ¿Qué protocolo de la capa de transporte sería el más adecuado para esta aplicación?

*   a) TCP, porque garantiza la entrega ordenada y fiable de todos los paquetes.
*   b) UDP, porque no establece una conexión previa y no realiza retransmisiones, ofreciendo una menor latencia.
*   c) TCP, porque su control de flujo evitará que se sature la red del usuario.
*   d) UDP, porque siempre utiliza menos ancho de banda que TCP.

<details>
  <summary>Ver Respuesta</summary>
  
  **Respuesta correcta: b)** 
  
  **Justificación:** Las aplicaciones en tiempo real como la videoconferencia o los juegos online priorizan la velocidad sobre la fiabilidad total. UDP es ideal para esto porque su naturaleza "no orientada a conexión" y la ausencia de mecanismos de retransmisión automática minimizan la latencia, que es el factor más crítico en este escenario.
</details>

---

### Pregunta 2

Un administrador de red necesita interconectar los 30 ordenadores de un departamento en una única red local (LAN). Quiere asegurarse de que la comunicación entre dos ordenadores de la LAN no afecte innecesariamente al resto de dispositivos, maximizando el rendimiento. En los años 90 se hubiera usado un hub, pero ¿qué dispositivo de red moderno debería usar para lograr este objetivo?

*   a) Un router, porque puede dirigir el tráfico a la subred correcta.
*   b) Un hub, porque es la forma más simple de conectar todos los dispositivos.
*   c) Un switch (conmutador), porque aprende las direcciones MAC de los dispositivos en cada puerto y solo reenvía las tramas al puerto de destino correspondiente.
*   d) Un módem, para dar acceso a Internet a la red.

<details>
  <summary>Ver Respuesta</summary>
  
  **Respuesta correcta: c)** 
  
  **Justificación:** A diferencia de un hub (dispositivo de capa 1) que repite cada trama por todos sus puertos (creando un único dominio de colisión), un switch (dispositivo de capa 2) es inteligente. Crea una tabla MAC y reenvía las tramas solo al puerto donde se encuentra el destinatario, segmentando la red en múltiples dominios de colisión y mejorando drásticamente el rendimiento y la eficiencia.
</details>

---

### Pregunta 3

Cuando envías un correo electrónico desde tu cliente de correo (ej. Outlook), el mensaje viaja a través de las diferentes capas del modelo OSI/TCP-IP. ¿En qué capa se añade la dirección IP del servidor de correo de destino al paquete?

*   a) Capa de Aplicación, junto con el contenido del mensaje.
*   b) Capa de Transporte, dentro de la cabecera TCP.
*   c) Capa de Red, dentro de la cabecera IP.
*   d) Capa de Enlace, dentro de la cabecera Ethernet.

<details>
  <summary>Ver Respuesta</summary>
  
  **Respuesta correcta: c)** 
  
  **Justificación:** La Capa de Red, cuyo protocolo principal es IP, es la responsable del enrutamiento global de paquetes entre el host de origen y el host de destino. Por lo tanto, es en la cabecera del paquete IP donde se colocan las direcciones IP de origen y destino, que identifican a las máquinas a nivel de toda la red.
</details>

---

### Pregunta 4

Una empresa tiene dos oficinas, una en Madrid y otra en Barcelona. Para conectar las dos oficinas a través de Internet como si estuvieran en la misma red local, utilizan una tecnología que crea un "túnel" cifrado entre ellas. ¿Qué tipo de red, según su extensión, están utilizando para interconectar sus dos LANs?

*   a) Una LAN (Local Area Network), porque los ordenadores se ven como si estuvieran en la misma red local.
*   b) Una WAN (Wide Area Network), porque están interconectando redes a través de una gran distancia geográfica usando la infraestructura pública de Internet.
*   c) Una MAN (Metropolitan Area Network), porque las ciudades son metrópolis.
*   d) Una PAN (Personal Area Network), porque la red es de uso privado para la empresa.

<details>
  <summary>Ver Respuesta</summary>
  
  **Respuesta correcta: b)**
  
  **Justificación:** Una WAN es una red que se extiende sobre un área geográfica amplia. Aunque el resultado final (una VPN) hace que las redes parezcan locales, la tecnología subyacente que se usa para conectar dos puntos geográficamente distantes (Madrid y Barcelona) es, por definición, una Red de Área Extendida (WAN).
</details>

---

### Pregunta 5

Un desarrollador está analizando un paquete con Wireshark y observa que la capa de transporte le pasa un segmento a la capa de red. La capa de red añade una cabecera y lo pasa a la capa de enlace, que a su vez añade su propia cabecera antes de enviarlo por el cable. ¿Cómo se denomina este proceso de añadir información de control (cabeceras) en cada capa?

*   a) Multiplexación.
*   b) Segmentación.
*   c) Encapsulación.
*   d) Demultiplexación.

<details>
  <summary>Ver Respuesta</summary>
  
  **Respuesta correcta: c)**
  
  **Justificación:** La encapsulación es el proceso fundamental en las arquitecturas de red por capas. Consiste en que cada capa toma los datos de la capa superior y les añade su propia cabecera (y a veces una cola, como en la capa de enlace) antes de pasarlo a la capa inferior. Es análogo a meter una carta (datos) en un sobre (cabecera TCP), y luego meter ese sobre en otro sobre más grande (cabecera IP).
</details>
