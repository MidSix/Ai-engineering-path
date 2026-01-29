# Notas Detalladas sobre el Tema 8: Intercambio de Datos en TCP

Este documento expande los conceptos presentados en las diapositivas del "Tema 8", proporcionando explicaciones más profundas sobre los mecanismos que TCP utiliza para garantizar una transferencia de datos fiable, eficiente y segura para la red.

---

### **1. El Reto: Transferencia Fiable sobre un Canal No Fiable**

Como vimos, la capa IP ofrece un servicio "best-effort" (no fiable). El trabajo de TCP es construir un servicio de transferencia de datos fiable encima de esto. Para lograrlo, cualquier protocolo, incluido TCP, debe implementar un mecanismo de **ARQ (Automatic Repeat reQuest)**, que se basa en tres pilares:

1.  **Detección de Errores:** Se usa un campo `checksum` para detectar si un segmento se ha corrompido en el camino. Si el checksum no coincide, el receptor descarta el segmento silenciosamente.
2.  **Realimentación (Feedback):** El receptor debe notificar al emisor qué datos ha recibido. TCP utiliza **confirmaciones positivas acumulativas (ACKs)**. Un ACK con número `N` significa "He recibido correctamente todos los bytes hasta `N-1`".
3.  **Retransmisión:** Si un segmento se pierde o se corrompe, el emisor debe volver a enviarlo. Para ello, cuando el emisor envía un segmento, inicia un **temporizador de retransmisión (RTO - Retransmission Timeout)**. Si el temporizador expira antes de recibir el ACK correspondiente, el emisor asume que el segmento se perdió y lo retransmite.

---

### **2. La Ventana Deslizante: Eficiencia en la Transmisión**

Un protocolo simple de "parada y espera" (enviar un paquete, esperar el ACK, enviar el siguiente) sería muy lento. TCP utiliza una técnica de **canalización (pipelining)** implementada mediante una **ventana deslizante** para ser mucho más eficiente.

*   **Concepto:** El emisor puede enviar múltiples segmentos (una "ventana" de datos) sin tener que esperar la confirmación de cada uno. Esto mantiene el "canal" de comunicación lleno y maximiza el rendimiento.
*   **Funcionamiento:**
    *   El emisor mantiene un registro de los números de secuencia que está enviando. La ventana cubre los segmentos que han sido enviados pero aún no confirmados.
    *   A medida que el receptor envía ACKs, el emisor "desliza" la ventana hacia adelante, permitiéndole enviar nuevos datos.
    *   Como los ACKs son **acumulativos**, si se pierde el ACK del paquete 2 pero llega el ACK del paquete 3, el emisor sabe que tanto el 2 como el 3 han sido recibidos correctamente.

#### **Optimización: Retransmisión Rápida (Fast Retransmit)**

Esperar a que un temporizador RTO expire puede ser muy lento, especialmente en redes con alta latencia (RTT alto). La retransmisión rápida es una optimización clave para recuperarse de pérdidas de paquetes mucho antes.

*   **Disparador:** Ocurre cuando el emisor recibe **tres ACKs duplicados**.
*   **¿Qué significa un ACK duplicado?** Supongamos que el emisor envía los segmentos 1, 2, 3, 4, 5. El segmento 2 se pierde, pero el 3, 4 y 5 llegan bien.
    1.  El receptor recibe el segmento 1 y envía `ACK 2`.
    2.  El segmento 2 nunca llega.
    3.  El receptor recibe el segmento 3. Como está esperando el 2, detecta un "hueco". Vuelve a enviar el último ACK que pudo confirmar: `ACK 2`.
    4.  El receptor recibe el segmento 4. Sigue esperando el 2, así que vuelve a enviar `ACK 2`.
    5.  El receptor recibe el segmento 5. Sigue esperando el 2, así que envía de nuevo `ACK 2`.
*   **La Inferencia:** Cuando el emisor ve llegar tres veces el mismo `ACK 2`, infiere con alta probabilidad que el segmento 2 se perdió, pero que la red sigue funcionando (ya que han llegado otros segmentos posteriores).
*   **La Acción:** En lugar de esperar el timeout, el emisor **retransmite inmediatamente** el segmento 2.

---

### **3. Control de Flujo: No Saturar al Receptor**

El control de flujo es el mecanismo que evita que un emisor rápido desborde el buffer de un receptor lento.

*   **Cómo funciona:** TCP utiliza el campo **Tamaño de Ventana** (`rwnd` o *Receive Window*) en su cabecera.
    1.  El receptor, en cada segmento que envía, usa este campo para "anunciar" la cantidad de espacio libre que le queda en su buffer de recepción.
    2.  El emisor mantiene un registro de esta ventana anunciada y no puede tener más bytes de datos "en vuelo" (enviados pero no confirmados) que el tamaño de `rwnd`.
*   **Ventana Cero y el Temporizador de Persistencia:**
    *   **El problema:** Si un receptor anuncia una `rwnd` de 0, el emisor se detiene. Más tarde, la aplicación del receptor lee datos, liberando espacio. El receptor envía un segmento con una nueva `rwnd` > 0. ¿Qué pasa si este "segmento de actualización de ventana" se pierde? El emisor esperaría indefinidamente (pensando que la ventana sigue en 0) y el receptor esperaría por más datos. Es un **abrazo mortal (deadlock)**.
    *   **La solución:** Cuando un emisor recibe una `rwnd` de 0, inicia un **temporizador de persistencia**. Si este temporizador expira sin haber recibido una actualización de ventana, el emisor envía una pequeña **sonda de ventana (window probe)** de 1 byte. Este paquete obliga al receptor a responder con un ACK que contendrá el tamaño de ventana actual, rompiendo así el bloqueo.

---

### **4. Control de Congestión: No Saturar la Red**

Este es uno de los mecanismos más importantes y complejos de TCP. Mientras que el control de flujo protege al receptor, el control de congestión protege a la red intermedia.

TCP asume que la principal causa de pérdida de paquetes en Internet es la **congestión en los routers**. Por lo tanto, usa la pérdida de paquetes como una señal para reducir su velocidad de transmisión.

*   **La Ventana de Congestión (`cwnd`):** El emisor mantiene una segunda variable de ventana, la `cwnd`. Esta ventana no la anuncia el receptor, sino que el propio emisor la calcula basándose en la congestión que percibe en la red.
*   **Regla de Envío:** La cantidad máxima de datos sin confirmar que un emisor puede tener en la red es: `min(rwnd, cwnd)`.

#### **El Algoritmo de Control de Congestión**

TCP ajusta su `cwnd` dinámicamente usando un algoritmo con varias fases:

1.  **Inicio Lento (Slow Start):**
    *   Al comenzar una conexión, `cwnd` se inicializa a un valor muy pequeño (típicamente 1 MSS - Maximum Segment Size).
    *   Por cada ACK recibido, `cwnd` se incrementa en 1 MSS.
    *   **Resultado:** La ventana de congestión **se duplica** en cada RTT (Round-Trip Time). Es un crecimiento **exponencial**, diseñado para encontrar rápidamente el ancho de banda disponible.
    *   Esta fase continúa hasta que se alcanza un umbral llamado `ssthresh` (slow start threshold).

2.  **Prevención de Congestión (Congestion Avoidance):**
    *   Una vez que `cwnd` supera `ssthresh`, el crecimiento se ralentiza para ser menos agresivo y evitar causar congestión.
    *   `cwnd` crece de forma **lineal**: se incrementa en 1 MSS por cada RTT.

3.  **Detección y Reacción ante la Congestión:**
    *   **Evento 1: Pérdida por Timeout.** Es una señal de congestión grave.
        *   Se actualiza `ssthresh` a `max(cwnd / 2, 2 * MSS)`.
        *   `cwnd` se resetea a **1 MSS**.
        *   La conexión vuelve a la fase de **Inicio Lento**.
    *   **Evento 2: Pérdida por 3 ACKs Duplicados (Fast Retransmit).** Es una señal de congestión más leve.
        *   Se actualiza `ssthresh` a `cwnd / 2`.
        *   `cwnd` se reduce a `ssthresh`.
        *   Se entra en la fase de **Recuperación Rápida (Fast Recovery)** para retransmitir el paquete perdido y luego se pasa directamente a **Prevención de Congestión**, evitando la penalización severa del Inicio Lento.

![Control de Congestión TCP](https://i.imgur.com/vjE7a8L.png)

---

### **5. Tráfico Interactivo y el Algoritmo de Nagle**

*   **El Problema de los "Tinygrams":** Aplicaciones como SSH o Telnet, donde cada pulsación de tecla puede generar un paquete, son muy ineficientes. Enviar 1 byte de datos requiere 40 bytes de cabeceras (20 IP + 20 TCP).
*   **La Solución (Algoritmo de Nagle):** Es una optimización del emisor para reducir este problema. Su regla es:
    > "Una conexión solo puede tener un único segmento pequeño sin confirmar en la red. Mientras se espera el ACK de ese segmento, cualquier otro dato pequeño que la aplicación quiera enviar debe ser acumulado en un buffer. Cuando llega el ACK, todos los datos acumulados se envían juntos en un único segmento más grande."
*   **Sinergia con ACKs Retardados:** Este algoritmo funciona muy bien con los **ACKs retardados** del receptor. El receptor retrasa el envío del ACK por si tiene datos propios que enviar (el "eco" del carácter). El algoritmo de Nagle, a su vez, espera a ese ACK para enviar más datos. El resultado es que, en el caso ideal de un SSH, la pulsación de tecla viaja en un paquete, y el eco de la tecla viaja de vuelta junto con el ACK, reduciendo el número de paquetes a la mitad.
*   **Inconvenientes:** Nagle introduce una pequeña latencia al esperar el ACK. En algunas aplicaciones en tiempo real (ciertos juegos), esta latencia es perjudicial y el algoritmo se desactiva explícitamente (`TCP_NODELAY`).
