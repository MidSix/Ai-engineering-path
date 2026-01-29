# Notas Detalladas sobre el Tema 4: TCP/IP y el Nivel de Enlace

Este documento expande los conceptos presentados en las diapositivas del "Tema 4", proporcionando explicaciones más profundas sobre la relación entre las direcciones IP y MAC y el funcionamiento del protocolo ARP.

---

### **1. Direcciones IP vs. Direcciones MAC: La Dicotomía Clave**

Para que la comunicación en red funcione, se necesitan dos tipos de direcciones, cada una con un propósito diferente. Entender esta diferencia es fundamental.

| Característica | Dirección IP (Capa 3 - Red)                                                                                           | Dirección MAC (Capa 2 - Enlace)                                                                                                             |
| :------------- | :-------------------------------------------------------------------------------------------------------------------- | :------------------------------------------------------------------------------------------------------------------------------------------ |
| **Analogía**   | La **dirección de tu casa** (Calle, número, ciudad).                                                                  | Tu **número de DNI** o pasaporte.                                                                                                           |
| **Propósito**  | Identificación **lógica** de un dispositivo en una red. Permite el enrutamiento a través de Internet.                 | Identificación **física** y única de una tarjeta de red (hardware). Permite la entrega en la red local.                                     |
| **Alcance**    | **Global**. Enruta paquetes entre redes distintas, desde tu casa hasta los servidores de Google en otro continente.   | **Local**. Solo tiene sentido dentro de la misma red local (LAN). Un router no propaga las direcciones MAC entre redes.                     |
| **Cambio**     | **Dinámica**. Cambia si te mueves de red. La IP de tu portátil en casa es diferente a la que tiene en la universidad. | **Permanente**. Está "quemada" en el hardware de fábrica. No cambia, sin importar a qué red te conectes.                                    |
| **Formato**    | 32 bits (IPv4) o 128 bits (IPv6).                                                                                     | 48 bits, representados en hexadecimal.                                                                                                      |
| **Estructura** | **Jerárquica**. Tiene una parte que identifica la red y otra que identifica al host dentro de esa red.                | **Plana**. Aunque los primeros 24 bits (OUI) identifican al fabricante, a efectos prácticos es un identificador único sin jerarquía de red. |

**Conclusión clave:** Necesitas ambas. La **dirección IP** se usa para llevar el paquete hasta la red local de destino (como el cartero que lleva una carta hasta tu calle). Una vez que el paquete llega al router de la red local, se usa la **dirección MAC** para hacer la entrega final a la tarjeta de red específica del dispositivo correcto (como el cartero que busca tu número de portal y buzón).

---

### **2. ARP (Address Resolution Protocol): El Traductor Indispensable**

Ahora viene la pregunta clave: Si la capa de Red (IP) sabe que quiere enviar un paquete a la IP `192.168.1.50`, pero la capa de Enlace (Ethernet) solo entiende de direcciones MAC, **¿cómo se "traduce" una dirección IP a una dirección MAC?**

Aquí es donde entra en juego **ARP (Address Resolution Protocol)**. Su única misión es responder a la pregunta: **"¿Qué dirección MAC corresponde a esta dirección IP?"**.

#### **El Proceso ARP en Acción: Un Ejemplo Detallado**

Siguiendo el ejemplo de las diapositivas (nogal conectando con pino), este es el flujo completo y detallado:

1.  **El Desencadenante (Usuario):** Un usuario en la máquina `nogal` ejecuta un comando para conectarse al servidor FTP `pino`.
2.  **Capa de Aplicación/Transporte:** El cliente FTP le pide a la capa de transporte (TCP) que cree una conexión con el servidor en la IP `210.53.23.32` y el puerto 21.
3.  **Capa de Red (Enrutamiento):** La capa de transporte le pasa un segmento a la capa de red (IP) con la instrucción: "envía esto a `210.53.23.32`". La capa IP consulta su tabla de enrutamiento y determina que `210.53.23.32` está en su misma red local. Por lo tanto, sabe que puede entregar el paquete directamente, sin pasarlo por un router.
4.  **El Dilema:** La capa de red le dice a la capa de enlace: "Prepara una trama Ethernet para enviar este paquete a `210.53.23.32`". Pero la capa de enlace se encuentra con un problema: para construir la trama, necesita la dirección MAC de destino. **Y no la conoce.** El paquete IP se queda en espera.
5.  **ARP al Rescate - La Petición (Request):** Se invoca el protocolo ARP. `nogal` construye un mensaje **ARP Request** que básicamente dice:
    *   "Hola a todos, soy el host con IP `210.53.23.10` y MAC `0f:9a:32:e3:09:8d`."
    *   "Necesito saber la dirección MAC del host que tenga la IP `210.53.23.32`."
6.  **Broadcast de la Petición:** Este mensaje ARP se encapsula en una trama de Ethernet. Como no se conoce la MAC de destino, se usa la dirección MAC de **broadcast**: `FF:FF:FF:FF:FF:FF`. Esto garantiza que **todos** los dispositivos en la red local recibirán y procesarán la trama.
7.  **Procesamiento de la Petición:** Todos los hosts de la red (`castaño`, `pino`, etc.) reciben la petición de ARP. Cada uno comprueba el campo "IP de destino" del mensaje.
    *   `castaño` ve que no es su IP, por lo que ignora y descarta el paquete.
    *   `pino` ve que **sí** es su IP. ¡Le están buscando a él!
8.  **ARP - La Respuesta (Reply):** `pino` prepara un mensaje **ARP Reply** que dice:
    *   "Hola `nogal`, yo soy `210.53.23.32`, y mi dirección MAC es `8e:9a:93:90:3a:8a`."
9.  **Unicast de la Respuesta:** Este mensaje de respuesta se encapsula en una trama Ethernet, pero esta vez se envía directamente a la dirección MAC de `nogal` (`0f:9a:32:e3:09:8d`), que `pino` aprendió de la petición original. Es una comunicación **unicast** (uno a uno).
10. **Resolución y Envío:** `nogal` recibe la respuesta. Ahora por fin conoce el mapeo: `210.53.23.32` → `8e:9a:93:90:3a:8a`.
    *   Guarda esta información en su **Caché ARP** para no tener que preguntar de nuevo la próxima vez.
    *   Saca el paquete IP original que tenía en espera, lo encapsula en una trama Ethernet con la MAC de destino `8e:9a:93:90:3a:8a` y lo envía.

¡Comunicación establecida!

---
### **3. La Caché ARP: La Memoria de la Red**

Enviar un broadcast por toda la red cada vez que se quiere enviar un paquete sería increíblemente ineficiente. Para evitarlo, los sistemas operativos mantienen una **Caché ARP**.

*   **¿Qué es?** Una pequeña tabla en memoria que almacena los mapeos de IP a MAC que ha aprendido recientemente.
*   **¿Cómo funciona?** Antes de enviar un ARP Request, un host siempre mira primero en su caché.
    *   **Si la IP está en la caché (ARP Hit):** ¡Genial! Usa la MAC almacenada directamente. No hay broadcast, no hay espera.
    *   **Si la IP no está en la caché (ARP Miss):** Solo entonces se inicia el proceso de ARP Request/Reply descrito anteriormente.
*   **Mantenimiento:** Las entradas en la caché no son para siempre. Tienen un tiempo de vida (TTL, *Time-To-Live*), que suele ser de unos minutos. Pasado ese tiempo, la entrada se borra. Esto asegura que si un dispositivo cambia su tarjeta de red (y por tanto su MAC), la información obsoleta no permanezca en la red indefinidamente.

Puedes inspeccionar la caché ARP de tu propio ordenador con el comando `arp -a` en la terminal.
