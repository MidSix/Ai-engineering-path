# Notas Detalladas sobre el Tema 3: Tecnologías del Nivel de Enlace

Este documento expande los conceptos presentados en las diapositivas del "Tema 3", proporcionando explicaciones más profundas sobre los protocolos de acceso al medio y las tecnologías Ethernet y Wi-Fi.

---

### **1. El Problema del Acceso Múltiple**

La capa de enlace se enfrenta a un desafío fundamental en medios compartidos (como un cable Ethernet antiguo o el aire en Wi-Fi): **¿cómo coordinar a múltiples estaciones que quieren hablar a la vez para evitar que sus mensajes se mezclen y se corrompan (colisionen)?**

Existen tres estrategias principales para resolverlo:

1.  **Particionamiento del Canal:** Se divide el canal para que cada nodo tenga su "trozo" reservado.
    *   **TDMA (Time Division Multiple Access):** Se divide el tiempo en ranuras y cada nodo tiene asignada una ranura para transmitir. Ineficiente si un nodo no tiene nada que enviar.
    *   **FDMA (Frequency Division Multiple Access):** Se divide la frecuencia en sub-canales y cada nodo usa uno. Es como las emisoras de radio. También es ineficiente si el espectro es limitado.
    *   **Ventaja:** Cero colisiones.
    *   **Desventaja:** Ineficiente. Si solo un nodo quiere transmitir, solo puede usar su pequeña porción del canal (ej. 1/N de la capacidad total), desperdiciando el resto.

2.  **Protocolos de Turnos:** Los nodos se turnan para usar el canal completo, pero de forma ordenada.
    *   **Polling:** Un nodo "maestro" pregunta (sondea) a los nodos "esclavos" uno por uno si tienen algo que transmitir.
    *   **Token Passing:** Un pequeño mensaje especial, el "token", se pasa de un nodo a otro. Solo el nodo que posee el token tiene permiso para transmitir.
    *   **Ventaja:** Más eficiente que el particionamiento y justo.
    *   **Desventaja:** Introduce latencia (hay que esperar tu turno) y tiene puntos únicos de fallo (si el maestro o el token fallan, la red se detiene).

3.  **Protocolos de Acceso Aleatorio (La estrategia de Internet):** No hay coordinación previa. Cada nodo intenta transmitir cuando quiere. Si hay una colisión, se detecta y se resuelve.
    *   **Filosofía:** "Es más fácil pedir perdón que pedir permiso".
    *   **Ventaja:** Simple, descentralizado y muy eficiente cuando la carga de la red es baja (pocos nodos transmitiendo).
    *   **Desventaja:** Las colisiones son posibles y suponen un problema a medida que la carga aumenta.
    *   **Ejemplos:** **ALOHA**, **CSMA**. **Ethernet** y **Wi-Fi** se basan en esta familia.

---

### **2. Ethernet y el Protocolo CSMA/CD**

Ethernet es la tecnología dominante para redes de área local (LAN) cableadas. Su protocolo de acceso al medio se llama **CSMA/CD (Carrier Sense Multiple Access / Collision Detection)**.

Vamos a desglosar su funcionamiento:

1.  **CS (Carrier Sense): "Escuchar antes de hablar"**. Una estación con una trama para enviar primero "escucha" el cable (el *carrier*) para comprobar si está libre. Si detecta que otra estación ya está transmitiendo, espera.
2.  **MA (Multiple Access):** Múltiples estaciones están conectadas al mismo medio compartido.
3.  **Transmisión:** Si el medio está libre, la estación empieza a transmitir su trama.
4.  **CD (Collision Detection): "Escuchar mientras se habla"**. Mientras la estación transmite, sigue escuchando el medio.
    *   **Si lo que escucha es idéntico a lo que transmite**, significa que todo va bien.
    *   **Si lo que escucha es diferente**, significa que otra estación empezó a transmitir a la vez y se ha producido una **colisión**.
5.  **Gestión de Colisión:**
    *   La estación que detecta la colisión detiene inmediatamente la transmisión de su trama y envía una **señal de Jamming** (un patrón de bits corto) para asegurarse de que todas las demás estaciones de la red se den cuenta de la colisión.
    *   Luego, la estación ejecuta el algoritmo de **Backoff Exponencial Binario**: espera un tiempo *aleatorio* antes de intentar retransmitir la trama desde el paso 1.
    *   **¿Por qué aleatorio?** Si ambas estaciones esperaran el mismo tiempo, volverían a colisionar. El tiempo aleatorio hace muy improbable que vuelvan a chocar.
    *   **¿Por qué exponencial?** Tras la primera colisión, el tiempo de espera se elige de un rango pequeño (ej. 0 o 1 "tiempo de trama"). Si vuelve a colisionar, el rango se duplica (0, 1, 2 o 3). Si vuelve a colisionar, se vuelve a duplicar (0 a 7), y así sucesivamente. Esto permite que la red se adapte: con pocas colisiones los reintentos son rápidos, pero si la red está muy congestionada, las estaciones esperan cada vez más para dar tiempo a que se libere el medio.

**Nota Importante:** El CSMA/CD es relevante en las redes Ethernet antiguas basadas en hubs o cables coaxiales. Las redes Ethernet modernas usan **switches (conmutadores)**, que crean un "microsegmento" por puerto. Cada dispositivo tiene su propio "cable" dedicado al switch, por lo que las colisiones ya no ocurren. Los dispositivos pueden operar en modo **Full-Duplex** (enviar y recibir a la vez).

---

### **3. La Trama Ethernet (IEEE 802.3)**

Una trama es el "paquete" de la capa de enlace. Su estructura es fundamental.
El tamaño minimo de la trama no cuenta el preambulo ni SFD. Solo se computa para el tamaño minimo los campos siguientes, si sumas veras que es 64bytes y ese es el tamaño de trama minimo.

| Campo                           | Longitud      | Descripción                                                                                                                                                                                                                                                |
| :------------------------------ | :------------ | :--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **Preámbulo**                   | 7 Bytes       | Secuencia de `101010...` que permite al receptor sincronizar su reloj con el del emisor. Es como una cuenta atrás antes del mensaje real.                                                                                                                  |
| **SFD (Start Frame Delimiter)** | 1 Byte        | Secuencia `10101011`. Marca el final del preámbulo y le dice al receptor: "¡La trama de verdad empieza ahora!".                                                                                                                                            |
| **Dirección MAC Destino**       | 6 Bytes       | La dirección física (48 bits) de la tarjeta de red de destino.                                                                                                                                                                                             |
| **Dirección MAC Origen**        | 6 Bytes       | La dirección física (48 bits) de la tarjeta de red que envía la trama.                                                                                                                                                                                     |
| **Tipo / Longitud**             | 2 Bytes       | Campo con doble propósito. Si su valor es `> 1535`, indica el **Tipo** de protocolo de la capa superior (ej. `0x0800` para IPv4). Si es `≤ 1500`, indica la **Longitud** del campo de datos.                                                               |
| **Datos**                       | 46-1500 Bytes | La carga útil (el "paquete" de la capa de red, ej. un paquete IP). Si es menor de 46 bytes, se añade **Relleno (Padding)** para alcanzar el tamaño mínimo de trama.                                                                                        |
| **FCS (Frame Check Sequence)**  | 4 Bytes       | Un código de **CRC (Cyclic Redundancy Check)**. El emisor calcula este valor basado en el contenido de la trama. El receptor hace el mismo cálculo. Si los valores no coinciden, significa que la trama se corrompió durante la transmisión y se descarta. |

**Tamaño Mínimo de Trama (64 bytes):** Es crucial para que CSMA/CD funcione. Una trama debe ser lo suficientemente larga como para que una estación siga transmitiendo cuando la señal de una colisión le llegue desde el punto más lejano de la red. Esto asegura que todas las colisiones sean detectadas.

---

### **4. Wi-Fi y el Protocolo CSMA/CA**

Wi-Fi (estándar IEEE 802.11) es la tecnología dominante para LANs inalámbricas. Usa el aire como medio compartido, lo que introduce nuevos problemas. Su protocolo es **CSMA/CA (Carrier Sense Multiple Access / Collision *Avoidance*)**.

#### **¿Por qué Evitar Colisiones en lugar de Detectarlas?**

Wi-Fi no puede usar la detección de colisiones (CD) de Ethernet por dos razones:
**PROBLEMA DEL NODO OCULTO -> RAZON POR LA QUE NO SE PUEDE USAR CD**
1.  **Es difícil detectar colisiones en radio:** En un cable, es fácil para una estación detectar que la señal que "oye" es más fuerte de la que está emitiendo. En radio, la propia transmisión de una estación puede ahogar cualquier otra señal que llegue, haciendo muy difícil detectar una colisión.
2. > **El Problema del Nodo Oculto:**
    *   Imagina tres estaciones: **A**, un Punto de Acceso (**B**) en el medio, y **C**.
    *   **A** y **C** están lejos, por lo que no se "oyen" entre sí, pero ambas oyen al Punto de Acceso **B**
    *   Si **A** escucha el medio, lo encontrará libre y empezará a transmitir a **B**.
    *   Si **C** hace lo mismo casi a la vez, también encontrará el medio libre y transmitirá a **B**.
    *   Sus señales colisionarán en el receptor (**B**), pero ni **A** ni **C** se darán cuenta de la colisión.

### En Resumen:

| Característica        | Ethernet (CSMA/CD)                             | Wi-Fi (CSMA/CA)                                       |
| :-------------------- | :--------------------------------------------- | :---------------------------------------------------- |
| **Medio**             | Cable (Predecible)                             | Aire (Caótico)                                        |
| **Capacidad**         | Puede escuchar mientras transmite              | **No puede** escuchar mientras transmite              |
| **Problema Clave**    | (No tiene uno equivalente)                     | Problema de la Estación Oculta                        |
| **Estrategia**        | **Reactiva**: Detecta la colisión y reacciona. | **Proactiva**: Intenta evitar que la colisión ocurra. |
| **Señal de Colisión** | Detección directa de energía en el cable.      | **Inferencia** por la falta de un ACK de confirmación |

#### **El Funcionamiento de CSMA/CA**

Como no podemos detectar colisiones, intentamos evitarlas a toda costa.

1.  **CSMA:** La estación escucha el canal. Si está ocupado, espera.
2.  **CA (Collision Avoidance):**
    *   Si el canal está libre, la estación no transmite inmediatamente. Espera un pequeño tiempo aleatorio (llamado **DIFS**). Si el canal sigue libre, entonces transmite. Este pequeño retardo aleatorio reduce la probabilidad de que dos estaciones empiecen a hablar a la vez.
    *   **ACKs (Acknowledgements):** Después de recibir una trama correctamente, el receptor envía una trama corta de confirmación (**ACK**). Si el emisor no recibe el ACK en un tiempo determinado, asume que la trama se perdió (probablemente por una colisión) y la retransmite usando el algoritmo de backoff exponencial.
	    * En caso de que el ACK sea enviado pero tambien se pierda por el camino el sender volvera a enviar la trama, el receiver al recibirla vera el numero de secuencia en la cabacera Wi-Fi y deduce que la trama es repetida, por tanto no la envia a capas superiores, la descarta y reenvia el ACK, asi hasta que el ACK sea recibido.

#### **RTS/CTS: Solución al Nodo Oculto**

Para tramas grandes, CSMA/CA puede usar un mecanismo opcional llamado **RTS/CTS (Request to Send / Clear to Send)**.

1.  La estación **A** quiere enviar una trama larga a **B**. En lugar de enviarla directamente, primero envía una trama muy corta **RTS** ("Pido permiso para enviar").
2.  El Punto de Acceso **B** responde con una trama corta **CTS** ("Permiso concedido").
3.  La estación "oculta" **C** no oye el RTS de **A**, pero sí oye el CTS de **B**. El CTS contiene la duración de la transmisión de A.
4.  Al oír el CTS, **C** sabe que el medio está reservado y se "calla" durante ese tiempo, evitando la colisión.
5.  Una vez **A** recibe el CTS, envía su trama de datos con la seguridad de que no habrá colisiones por nodos ocultos.

---

### **5. Seguridad en Wi-Fi: De WEP a WPA3**

El aire es un medio abierto y fácil de escuchar. La seguridad es crítica.

*   **WEP (Wired Equivalent Privacy):** El primer estándar. **Totalmente inseguro y roto**. Utiliza una clave estática y algoritmos criptográficos débiles. **Nunca debe usarse**.
*   **WPA (Wi-Fi Protected Access):** Fue el "parche" de emergencia para WEP. Introdujo **TKIP (Temporal Key Integrity Protocol)**, que cambiaba las claves dinámicamente, solucionando el fallo principal de WEP. Fue una medida de transición.
*   **WPA2:** Ha sido el estándar durante muchos años. Reemplazó el débil TKIP por **AES**, un algoritmo de cifrado muy fuerte y considerado seguro a día de hoy. Es el mínimo nivel de seguridad aceptable. Su principal debilidad es que es vulnerable a ataques de fuerza bruta si se usa una contraseña simple.
*   **WPA3:** El estándar más reciente. Ofrece una protección mucho mayor incluso con contraseñas simples, haciendo los ataques de fuerza bruta mucho más difíciles. También introduce mejoras de seguridad para redes públicas/abiertas. Es el estándar recomendado actualmente.
