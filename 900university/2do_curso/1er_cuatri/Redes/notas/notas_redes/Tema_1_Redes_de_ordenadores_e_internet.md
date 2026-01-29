# Notas Detalladas sobre el Tema 1: Redes de Ordenadores e Internet

Este documento expande los conceptos presentados en las diapositivas del "Tema 1", proporcionando explicaciones más profundas para facilitar la comprensión sin necesidad de una clase magistral.

---

### **1. ¿Qué es Internet? Una Visión Detallada**

Las diapositivas definen Internet como una "Red de redes" y una "Autopista de la Información". Vamos a desglosar esto.

#### **Servicios de Internet: Conexión vs. No Conexión**

La presentación menciona dos tipos de servicio que Internet proporciona, y estos son cruciales para entender cómo funcionan las aplicaciones.

*   **Servicio Fiable y Orientado a Conexión (TCP - Protocolo de Control de Transmisión)**
    *   **¿Qué significa "Orientado a Conexión"?** Imagina que quieres tener una conversación telefónica. Primero, tienes que establecer una conexión marcando el número y esperando a que la otra persona responda. Durante la llamada, el canal de comunicación es exclusivo para ustedes. Al terminar, cuelgan y la conexión se cierra. TCP funciona de manera análoga. Antes de que se envíen datos, se realiza un "saludo de tres vías" (three-way handshake) para establecer una conexión virtual entre el cliente y el servidor.
    *   **¿Qué significa "Fiable"?** TCP garantiza que todos los datos enviados llegarán al destino, en el orden correcto y sin errores. Si un paquete de datos se pierde en el camino, TCP lo detecta y solicita que se reenvíe. También gestiona el "control de flujo" (para no saturar al receptor) y el "control de congestión" (para no saturar la red).
    *   **Ejemplos de uso:** Navegación web (HTTP/S), envío de correos electrónicos (SMTP), transferencia de archivos (FTP). En estas aplicaciones, es fundamental que la información llegue completa y sin errores.

*   **Servicio No Fiable y No Orientado a Conexión (UDP - Protocolo de Datagramas de Usuario)**
    *   **¿Qué significa "No Orientado a Conexión"?** Piensa en enviar una carta por correo postal. Simplemente escribes la dirección en el sobre y la echas al buzón. No estableces una conexión previa con el destinatario. Cada carta (o paquete de datos) se envía de forma independiente.
    *   **¿Qué significa "No Fiable"?** UDP no ofrece garantías. Los paquetes pueden perderse, llegar duplicados o en un orden diferente al que se enviaron. No hay retransmisiones automáticas. Es un servicio de "mejor esfuerzo" (*best-effort*) -> "Hace lo mejor que puede pero no garantiza nada, el esfuerzo no garantiza el exito jaja".
    *   **¿Por qué usarlo?** Porque es mucho más rápido y ligero que TCP. No tiene la sobrecarga de establecer conexiones ni de gestionar la fiabilidad.
    *   **Ejemplos de uso:** Streaming de vídeo (como YouTube o Netflix), videojuegos online, llamadas de voz sobre IP (VoIP). En estas aplicaciones, la velocidad y la baja latencia son más importantes que la pérdida ocasional de un paquete. Perder un fotograma en un vídeo es preferible a detener la reproducción para esperar a que llegue un paquete retrasado.

---

### **2. Tipos de Redes: LAN, MAN y WAN**

Las diapositivas clasifican las redes por su longitud. Aquí hay más detalles sobre su propósito y tecnología.

*   **LAN (Local Area Network - Red de Área Local)**
    *   **Alcance:** Pequeña área geográfica como una casa, una oficina, un campus universitario.
    *   **Propiedad:** Generalmente es propiedad privada de la organización que la utiliza.
    *   **Tecnologías:** Ethernet (cableada) y Wi-Fi (inalámbrica) son las más comunes. Las velocidades son muy altas (1 Gbps o más) y la latencia es muy baja.
    *   **Concepto clave:** El medio de transmisión (los cables o el espectro de radio) es compartido por todos los dispositivos en la LAN.

*   **WAN (Wide Area Network - Red de Área Extendida)**
    *   **Alcance:** Cubre grandes áreas geográficas, como un país, un continente o incluso el mundo entero. Internet es el ejemplo más grande de una WAN.
    *   **Propiedad:** Las WAN suelen ser operadas por proveedores de servicios de telecomunicaciones (como Telefónica, Orange, Vodafone). Las empresas "alquilan" circuitos o conexiones a estos proveedores.
    *   **Tecnologías:** Utilizan tecnologías de enlace de larga distancia como fibra óptica submarina, enlaces por satélite, etc. Las velocidades son más variables y la latencia es significativamente mayor que en una LAN.

*   **MAN (Metropolitan Area Network - Red de Área Metropolitana)**
    *   **Alcance:** Es un punto intermedio. Cubre un área más grande que una LAN pero más pequeña que una WAN, como una ciudad.
    *   **Ejemplo:** Un proveedor de cable que ofrece servicio a toda una ciudad está operando una MAN. A menudo, se utilizan para interconectar varias LANs de una misma organización repartidas por una ciudad.

---

### **3. Conmutación de Circuitos vs. Conmutación de Paquetes**

Esta es una de las distinciones más importantes en las redes.

*   **Conmutación de Circuitos**
    *   **Analogía:** Una llamada telefónica tradicional.
    *   **Funcionamiento:** Se reserva un camino dedicado (un "circuito") a través de la red desde el origen hasta el destino. Todos los recursos necesarios para la comunicación (ancho de banda, capacidad de los conmutadores) se reservan *antes* de que comience la comunicación y se mantienen durante toda la sesión.
    *   **Pros:** Calidad de servicio garantizada. Una vez establecido el circuito, el rendimiento (ancho de banda, retardo) es constante.
    *   **Contras:** Muy ineficiente. Si no se están enviando datos, los recursos reservados se desperdician. Es como mantener una línea telefónica abierta sin que nadie hable.
    *   **Multiplexación (FDM y TDM):** Para usar los enlaces de manera más eficiente, se pueden compartir.
		En estos dos casos se coge el canal y se divide en porciones, la logistica que se sigue para obtener estas porciones son por tiempo y frecuencia como se especifica abajo.
        *   **FDM (Frequency-Division Multiplexing):** Se divide el espectro de frecuencia del enlace en canales más pequeños, y a cada comunicación se le asigna un canal. Es como si diferentes emisoras de radio emitieran a la vez por el aire, cada una en su propia frecuencia.
        *   **TDM (Time-Division Multiplexing):** Se divide el tiempo en pequeñas ranuras ("slots"). A cada comunicación se le asigna una ranura de tiempo de forma repetitiva. Por ejemplo, la comunicación 1 usa la ranura 1, la 2 usa la 2, etc., y luego vuelve a empezar.

*   **Conmutación de Paquetes**
    *   **Analogía:** El sistema de correo postal.
    *   **Funcionamiento:** Los mensajes se dividen en trozos más pequeños llamados **paquetes**. Cada paquete contiene información de la dirección de destino. Los paquetes se envían a la red uno por uno y viajan de forma independiente. Los routers (las "oficinas de correos" de la red) examinan la dirección de cada paquete y lo reenvían al siguiente salto hacia su destino.
    *   **Pros:** Mucho más eficiente y flexible. Los recursos de la red se comparten "bajo demanda". Si un usuario no envía datos, no consume ancho de banda, que puede ser utilizado por otros. Permite que muchos más usuarios compartan la red simultáneamente.
    *   **Contras:** No hay garantía de rendimiento. Como los recursos son compartidos, pueden producirse congestiones y retardos si muchos paquetes intentan usar el mismo enlace a la vez.
    *   **Este es el modelo que utiliza Internet.**

---

### **4. Profundizando en la Conmutación de Paquetes**

#### **Retardos en la Red**

La diapositiva 9 introduce 4 tipos de retardo. Entenderlos es clave para analizar el rendimiento de la red. Pensemos en una caravana de 10 coches (los bits de un paquete) que tienen que pasar por un peaje (un router) para llegar a su destino por una autopista (un enlace).

1.  **Retardo de Procesamiento:** Es el tiempo que tarda el operario del peaje (router) en examinar el destino de cada coche y decidir por qué barrera enviarlo. Suele ser muy pequeño, del orden de microsegundos.
2.  **Retardo de Cola:** Si hay muchos coches llegando al peaje a la vez, se formará una cola. Este es el tiempo que un coche (paquete) pasa esperando en el buffer de salida del router hasta que le toca ser atendido. Este retardo es variable y es la principal fuente de *jitter* (variación del retardo). Si la cola se llena, los paquetes que lleguen se descartarán (**pérdida de paquetes**).
3.  **Retardo de Transmisión:** Es el tiempo que se tarda en poner todos los coches de la caravana en la autopista. Depende del tamaño del paquete (longitud de la caravana) y del ancho de banda del enlace (cuántos carriles tiene la autopista), basicamente es el tiempo que se demora el router en coger todos los bits del paquete que le ha llegado y volcarlos, ponerlos, redirigirlos, disponerlos en la interfaz fisica por la que saldran los bits del paquete al siguiente router, al siguiente salto.
    *   *Fórmula: Retardo de Transmisión = Tamaño del Paquete (bits) / Ancho de Banda (bits por segundo)*
4.  **Retardo de Propagación:** Es el tiempo que tarda el primer coche en viajar desde el peaje hasta el siguiente destino a través de la autopista. Depende de la distancia física y de la velocidad de la luz en el medio (cable, fibra, aire). **O sea, el tiempo que tarda el primer bit de un punto A a un punto B**
    *   *Fórmula: Retardo de Propagación = Distancia (metros) / Velocidad de Propagación (metros por segundo)*

>https://www.tkn.tu-berlin.de/teaching/rn/animations/propagation/ El profesor deja esta pagina para ver la diferencia entre retardo de transferencia y retardo de propagacion. Explico -> La pagina muestra el envio de **UN SOLO PAQUETE** de un Sender a un Receiver y el retardo de propagacion es simplemente el tiempo que demora en llegar el primer bit del paquete y el retardo de transmision es el tiempo que se demora en volva todos los bits del paquete a la interfaz de salida del router, para eso es indispensable que todos los bits del paquete lleguen por supuesto. Por eso el retardo de transmision depende de la cantidad de bits que pueda enviar por segundo, o sea, del ancho de banda y del tamaño del archivo, a mayor tamaño más bits tengo que enviar y a menor tamaño menos bits tengo que enviar.

El **retardo total** de un paquete en un salto es la suma de estos cuatro.

El retardo total de un paquete desde que sale de su emisor y llega a su receptor destino es la suma de todos los retardos totales de cada salto.
#### **Redes de Datagramas vs. Circuitos Virtuales**

Son dos enfoques dentro de la conmutación de paquetes.

*   **Redes de Datagramas:** Es el enfoque de Internet. No se establece una ruta fija. Cada paquete (datagrama) se enruta de forma independiente basándose en su dirección de destino. Los routers no guardan información sobre las conexiones. Esto hace que la red sea muy robusta: si un router falla, los siguientes paquetes simplemente se enviarán por otra ruta.
*   **Redes de Circuito Virtual (CV):** Es un híbrido. Antes de enviar datos, se establece una ruta específica a través de la red (el "circuito virtual"). A esta conexión se le asigna un número. Los paquetes solo llevan este número de CV, no la dirección completa. Los routers mantienen una tabla de estado para saber por dónde reenviar los paquetes de cada CV. Es más rápido en el reenvío de paquetes (solo se mira un número), pero menos robusto que las redes de datagramas.

---

### **5. El Modelo de Referencia OSI: Una Guía Estructurada**

El modelo OSI es un marco conceptual que divide las funciones de una red en 7 capas. Es una guía, no una implementación estricta (la arquitectura de Internet, TCP/IP, es más práctica y tiene menos capas).

*   **Capa 1: Nivel Físico**
    *   **Función:** Transmitir bits (0s y 1s) a través del medio físico.
    *   **Detalles:** Define las especificaciones eléctricas y físicas: voltajes, tipos de conectores (como el RJ45 de Ethernet), cómo se codifica un 1 y un 0 (por ejemplo, 5V puede ser un 1 y 0V un 0), y la sincronización a nivel de bit.
    *   **PDU (Unidad de Datos de Protocolo):** Bit.

*   **Capa 2: Nivel de Enlace**
    *   **Función:** Proporcionar una comunicación fiable entre dos nodos *directamente conectados* (un salto).
    *   **Detalles:** Agrupa los bits en **tramas** (*frames*). Gestiona el acceso al medio (quién puede transmitir en un canal compartido) a través de la subcapa **MAC (Medium Access Control)**. Proporciona direccionamiento físico (direcciones MAC). También puede realizar detección de errores.
    *   **PDU:** Trama.

*   **Capa 3: Nivel de Red**
    *   **Función:** Realizar el enrutamiento de paquetes desde el origen hasta el destino final a través de múltiples redes (múltiples saltos).
    *   **Detalles:** Se encarga del direccionamiento lógico (direcciones IP). Decide la mejor ruta para los paquetes.
    *   **PDU:** Paquete (o Datagrama).

*   **Capa 4: Nivel de Transporte**
    *   **Función:** Proporcionar una comunicación extremo a extremo entre *procesos* (aplicaciones) en los hosts de origen y destino.
    *   **Detalles:** Aquí es donde viven TCP y UDP. Se encarga de la segmentación de los datos de la aplicación, el control de errores (TCP), el control de flujo (TCP) y la **multiplexación** mediante el uso de **números de puerto** (un puerto es un identificador para una aplicación específica, ej. el puerto 80 para web).
    *   **PDU:** Segmento (TCP) o Datagrama (UDP).

*   **Capa 5: Nivel de Sesión**
    *   **Función:** Establecer, gestionar y terminar las sesiones de comunicación entre aplicaciones.
    *   **Detalles:** Se encarga de la sincronización. Por ejemplo, en una transferencia de archivos grande, puede insertar "puntos de control" para que si la transferencia se interrumpe, se pueda reanudar desde el último punto de control en lugar de desde el principio.

*   **Capa 6: Nivel de Presentación**
    *   **Función:** Asegurarse de que los datos enviados por la aplicación de un sistema puedan ser leídos por la aplicación de otro sistema.
    *   **Detalles:** Actúa como un "traductor". Se encarga de la codificación de caracteres (ej. ASCII, UTF-8), la compresión de datos y el cifrado (aunque esto también se hace en otras capas). Un ejemplo es la conversión de datos **Little-endian vs. Big-endian** (diferentes formas en que los ordenadores almacenan los bytes de un número en memoria).

*   **Capa 7: Nivel de Aplicación**
    *   **Función:** Proporcionar los servicios de red directamente al usuario o a las aplicaciones.
    *   **Detalles:** Es la capa con la que interactuamos. Aquí se encuentran protocolos como HTTP (web), SMTP (e-mail), FTP (archivos), DNS (resolución de nombres), etc.
    *   **PDU:** Datos (o Mensaje).
