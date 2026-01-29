# Notas Detalladas sobre el Tema 7: Capa de Transporte (UDP y TCP)

Este documento expande los conceptos presentados en las diapositivas del "Tema 7", proporcionando explicaciones más profundas sobre los dos protocolos principales de la capa de transporte: el rápido y simple UDP, y el fiable y complejo TCP.

---

### **1. UDP (User Datagram Protocol): El Sprinter Minimalista**

UDP es un protocolo de transporte que ofrece un servicio mínimo sobre la capa IP. Su filosofía es la velocidad y la simplicidad.

**Características Principales:**

*   **No Orientado a Conexión:** Al igual que IP, UDP no establece una conexión previa. Simplemente añade su cabecera a los datos de la aplicación y los entrega a la capa IP para que los envíe.
*   **No Fiable:** UDP no garantiza nada. No sabe si un datagrama se ha perdido, ha llegado duplicado o en desorden. No hay ACKs ni retransmisiones. Hereda directamente el servicio "best-effort" de IP.
*   **Ligero:** Su cabecera es de tan solo 8 bytes, un overhead mínimo.

**Sus dos funciones clave son:**

1.  **Multiplexación/Demultiplexación por Puertos:** Permite que diferentes aplicaciones en un mismo host puedan enviar y recibir datos simultáneamente. La dirección IP lleva el datagrama a la máquina correcta; el número de puerto lo lleva a la aplicación correcta.
2.  **Comprobación de Errores (Opcional pero Estándar):** El campo `Checksum` permite al receptor verificar si los datos se han corrompido durante la transmisión.

#### **¿Cuándo Usar UDP?**

*   **Aplicaciones Sensibles a la Latencia:** Streaming de vídeo, voz sobre IP (VoIP), juegos online. En estas aplicaciones, perder un paquete es preferible a esperar una retransmisión que llegaría demasiado tarde. La velocidad es más importante que la fiabilidad total.
*   **Aplicaciones Simples de Consulta-Respuesta:** DNS (resolución de nombres), DHCP (configuración de red). El cliente envía una petición y, si no obtiene respuesta en un tiempo, simplemente la vuelve a enviar. Establecer y cerrar una conexión TCP para una única petición y respuesta sería un desperdicio de recursos.
*   **Transmisiones a Múltiples Destinos:** UDP soporta el envío de paquetes broadcast y multicast, algo que TCP (que es estrictamente punto a punto) no puede hacer.

#### **La Cabecera UDP**

Su cabecera de 8 bytes es muy simple:

| Campo | Longitud | Descripción |
| :--- | :--- | :--- |
| **Puerto Origen** | 2 Bytes | El puerto de la aplicación que envía el datagrama. |
| **Puerto Destino** | 2 Bytes | El puerto de la aplicación que debe recibir el datagrama. |
| **Longitud** | 2 Bytes | La longitud en bytes del datagrama UDP completo (cabecera + datos). |
| **Checksum** | 2 Bytes | Suma de comprobación para detectar errores. **Cubre la cabecera UDP, los datos UDP y una "pseudo-cabecera IP"** (que incluye las IPs de origen y destino). Esto no solo detecta corrupción de bits, sino que también confirma que el paquete ha llegado al destino correcto. |

---

### **2. TCP (Transmission Control Protocol): El Mariscal de Campo Fiable**

TCP es el protocolo de transporte más utilizado en Internet. Su objetivo es proporcionar un **flujo de datos fiable y ordenado** sobre el servicio no fiable de IP.

**Características Principales:**

*   **Orientado a Conexión:** Antes de enviar datos, cliente y servidor realizan un **"saludo de tres vías" (Three-Way Handshake)** para establecer una conexión lógica y sincronizar sus parámetros.
*   **Fiable:** TCP garantiza que todos los datos se reciben sin errores, sin duplicados y en el orden correcto.
*   **Full-Duplex:** Los datos pueden fluir en ambas direcciones al mismo tiempo dentro de la misma conexión.
*   **Control de Flujo:** Asegura que un emisor rápido no sature a un receptor lento.
*   **Control de Congestión:** Intenta evitar la sobrecarga de la propia red.

#### **La Cabecera TCP**

La cabecera TCP es mucho más compleja que la de UDP (20 bytes como mínimo).

![Cabecera TCP](https://i.imgur.com/xO4sH6v.png)

*   **Puerto Origen/Destino (16 bits cada uno):** Igual que en UDP, para la multiplexación.
*   **Número de Secuencia (32 bits):** TCP ve los datos como un flujo de bytes. Este campo indica la posición en el flujo del primer byte de datos de este segmento. Es el pilar de la entrega ordenada y la retransmisión.
*   **Número de ACK (32 bits):** Indica el **siguiente** número de secuencia que el receptor espera recibir. Es acumulativo, es decir, si se envía un ACK de 500, significa que se han recibido correctamente todos los bytes hasta el 499.
*   **Longitud de Cabecera (4 bits):** Igual que en IP, indica el tamaño de la cabecera en palabras de 32 bits.
*   **Flags (Banderas de Control):** Bits individuales que gestionan el estado de la conexión.
    *   `SYN`: Se usa para **Sincronizar** los números de secuencia e iniciar una conexión.
    *   `ACK`: Indica que el campo "Número de ACK" es válido.
    *   `FIN`: Se usa para **Finalizar** la conexión de forma ordenada.
    *   `RST`: Se usa para **Resetear** la conexión de forma abrupta por un error.
    *   `PSH`: Pide al receptor que entregue los datos a la aplicación inmediatamente.
    *   `URG`: Indica que hay datos "urgentes".
*   **Tamaño de Ventana (16 bits):** Utilizado para el **control de flujo**. El receptor lo usa para anunciar cuánto espacio libre tiene en su buffer. El emisor no puede enviar más datos que el tamaño de esta ventana.
*   **Checksum (16 bits):** Igual que en UDP, protege de errores la cabecera, los datos y una pseudo-cabecera IP.

#### **Gestión de la Conexión TCP**

**1. Establecimiento: El Saludo de Tres Vías (Three-Way Handshake)**

1.  **Cliente → Servidor: [SYN]**
    *   El cliente envía un segmento con el flag `SYN` activado.
    *   Elige un Número de Secuencia Inicial aleatorio (ISN), por ejemplo, `Seq=X`.
    *   Significado: *"Hola, quiero conectar. Mi número de secuencia empieza en X."*
2.  **Servidor → Cliente: [SYN, ACK]**
    *   El servidor responde con un segmento con los flags `SYN` y `ACK` activados.
    *   Elige su propio ISN, por ejemplo, `Seq=Y`.
    *   Confirma el SYN del cliente poniendo `Ack=X+1`.
    *   Significado: *"¡Claro! Acepto tu conexión. Mi secuencia empieza en Y, y he recibido tu secuencia X, así que ahora espero el byte X+1."*
3.  **Cliente → Servidor: [ACK]**
    *   El cliente envía un último segmento con el flag `ACK` activado.
    *   Confirma el SYN del servidor poniendo `Ack=Y+1`.
    *   Significado: *"¡Entendido! He recibido tu secuencia Y, ahora espero el byte Y+1. La conexión está establecida."*

**2. Finalización: El Cierre en Cuatro Pasos**

TCP cierra cada dirección del flujo de forma independiente (*half-close*).

1.  **Extremo A → Extremo B: [FIN]**
    *   A ha terminado de enviar datos y envía un `FIN`.
    *   Significado: *"Por mi parte, he terminado de hablar."*
2.  **Extremo B → Extremo A: [ACK]**
    *   B confirma la recepción del FIN.
    *   Significado: *"Recibido. Sé que no vas a hablar más."*. En este punto, B todavía podría seguir enviando datos a A si quisiera.
3.  **Extremo B → Extremo A: [FIN]**
    *   Cuando B también ha terminado de enviar sus datos, manda su propio `FIN`.
    *   Significado: *"Ahora sí, yo también he terminado de hablar."*
4.  **Extremo A → Extremo B: [ACK]**
    *   A confirma la recepción del FIN de B. La conexión está cerrada.

*   **Estado TIME_WAIT:** Después de enviar el último ACK, el extremo A no cierra la conexión inmediatamente, sino que entra en el estado `TIME_WAIT`. Espera un tiempo (normalmente 2 minutos) para asegurarse de que su ACK final ha llegado y para dar tiempo a que cualquier paquete antiguo y retrasado de esa conexión "muera" en la red, evitando que interfiera con una nueva conexión futura que pudiera reutilizar los mismos puertos.

#### **MSS (Maximum Segment Size)**

*   El **MTU (Maximum Transmission Unit)** es el tamaño máximo del paquete de la capa de enlace (ej. 1500 bytes en Ethernet).
*   El **MSS** es el tamaño máximo de los **datos** que un segmento TCP puede contener.
*   Durante el Three-Way Handshake, cada extremo anuncia su MSS. Generalmente, se calcula como `MSS = MTU - 40` (20 bytes para la cabecera IP + 20 para la cabecera TCP).
*   El objetivo es construir segmentos TCP que, una vez añadida la cabecera IP, quepan perfectamente en una trama de enlace, **evitando así la fragmentación IP**, que es un proceso ineficiente.
