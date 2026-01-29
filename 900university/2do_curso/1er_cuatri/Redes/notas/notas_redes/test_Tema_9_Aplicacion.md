# Test de Autoevaluación - Tema 9: Capa de Aplicación

Este test contiene preguntas de opción múltiple diseñadas para evaluar la comprensión de los protocolos y arquitecturas fundamentales de la capa de aplicación, como HTTP, DNS, SMTP, POP3/IMAP y P2P.

---

### Pregunta 1

Cuando tu navegador solicita la página web `http://www.ejemplo.com/index.html`, ocurren varios pasos. ¿Cuál de las siguientes secuencias describe mejor el proceso inicial?

*   a) El navegador abre una conexión TCP con `www.ejemplo.com` en el puerto 53 y luego envía una petición HTTP.
*   b) El navegador primero realiza una consulta DNS para obtener la dirección IP de `www.ejemplo.com`, y luego establece una conexión TCP en el puerto 80 con esa IP para enviar la petición HTTP.
*   c) El navegador envía una petición HTTP directamente a la dirección IP `www.ejemplo.com`, ya que los nombres de dominio son solo alias.
*   d) El navegador utiliza el protocolo UDP para enviar la petición HTTP, ya que es más rápido que TCP.

<details>
  <summary>Ver Respuesta</summary>
  
  **Respuesta correcta: b)** 
  
  **Justificación:** Las comunicaciones a nivel de red se realizan con direcciones IP, no con nombres de dominio. Por lo tanto, el primer paso indispensable es traducir el nombre del host (`www.ejemplo.com`) a su dirección IP correspondiente. Esta tarea la realiza el sistema de nombres de dominio (DNS). Una vez obtenida la IP, el navegador puede iniciar la comunicación a través del protocolo de la capa de transporte (TCP para HTTP) usando el puerto estándar para el servicio (puerto 80 para HTTP).
</details>

---

### Pregunta 2

Tu cliente de correo (ej. Thunderbird u Outlook) está configurado para descargar los correos de un servidor y borrarlos de allí una vez descargados. Esta configuración es típica de un protocolo de acceso a correo específico. ¿Cuál es y cuál es su principal característica?

*   a) IMAP, que sincroniza el estado de los correos entre el cliente y el servidor.
*   b) SMTP, que se utiliza para leer correos del servidor.
*   c) POP3, que opera en un modo simple de "descargar y (opcionalmente) borrar", tratando al servidor como un simple buzón temporal.
*   d) HTTP, que se usa para mostrar los correos en una interfaz web.

<details>
  <summary>Ver Respuesta</summary>
  
  **Respuesta correcta: c)** 
  
  **Justificación:** POP3 (Post Office Protocol v3) fue diseñado con un modelo de operación simple y sin estado entre sesiones. El ciclo típico es: conectar, autenticar, descargar todos los correos nuevos, borrarlos del servidor y desconectar. Esto contrasta con IMAP (Internet Message Access Protocol), que está diseñado para mantener los correos en el servidor y sincronizar su estado (leído, no leído, carpetas, etc.) a través de múltiples clientes. SMTP solo se usa para *enviar* correo.
</details>

---

### Pregunta 3

El DNS (Domain Name System) es una base de datos jerárquica y distribuida. Cuando tu servidor DNS local no conoce la IP de `www.servidor-nuevo.com`, inicia una serie de consultas. ¿Qué tipo de consulta realiza típicamente tu servidor local al servidor raíz de DNS?

*   a) Una consulta recursiva, pidiéndole al servidor raíz que le devuelva la respuesta final.
*   b) Una consulta iterativa, donde el servidor raíz no le da la respuesta final, pero le indica cuál es el siguiente servidor al que debe preguntar (el servidor TLD para `.com`).
*   c) Envía un paquete de broadcast a toda la red para ver si alguien conoce la dirección.
*   d) Contacta directamente al servidor autoritativo de `servidor-nuevo.com`.

<details>
  <summary>Ver Respuesta</summary>
  
  **Respuesta correcta: b)** 
  
  **Justificación:** La consulta entre el cliente DNS (tu PC) y tu servidor DNS local suele ser recursiva (el cliente quiere la respuesta final). Sin embargo, la comunicación entre servidores DNS en la jerarquía (local -> raíz -> TLD -> autoritativo) es típicamente iterativa. El servidor raíz no conoce todas las IPs, pero sabe quiénes son los responsables de cada dominio de nivel superior (TLD). Por tanto, al ser preguntado por `www.servidor-nuevo.com`, responde con una referencia al servidor TLD de `.com`, al que el servidor local deberá preguntar a continuación.
</details>

---

### Pregunta 4

¿Cuál es la principal ventaja de HTTP/1.1 con conexiones persistentes sobre las conexiones no persistentes de HTTP/1.0?

*   a) Las conexiones persistentes utilizan UDP, que es más rápido.
*   b) Reduce la latencia al reutilizar la misma conexión TCP para múltiples peticiones (ej. la página HTML y sus imágenes), evitando la sobrecarga de establecer y cerrar una nueva conexión TCP para cada objeto.
*   c) Permite que el servidor almacene información del usuario entre diferentes visitas.
*   d) Comprime las cabeceras HTTP, reduciendo el consumo de ancho de banda.

<details>
  <summary>Ver Respuesta</summary>
  
  **Respuesta correcta: b)**
  
  **Justificación:** Establecer una conexión TCP requiere un "three-way handshake", que introduce un retardo de al menos un RTT (Round-Trip Time). En HTTP/1.0, cada objeto de una página web (HTML, cada imagen, cada CSS) requería su propia conexión TCP, multiplicando esta latencia. Las conexiones persistentes (por defecto en HTTP/1.1) solucionan esto manteniendo la conexión TCP abierta tras una petición, permitiendo que el cliente la reutilice para solicitar los objetos siguientes de forma mucho más rápida.
</details>

---

### Pregunta 5

En el contexto de las arquitecturas de aplicación, ¿cuál es una característica fundamental que diferencia un modelo Peer-to-Peer (P2P) de un modelo cliente-servidor tradicional?

*   a) En P2P, todos los participantes deben tener una dirección IP estática.
*   b) El modelo P2P se basa exclusivamente en el protocolo UDP.
*   c) En P2P, los participantes (peers) actúan simultáneamente como clientes y servidores, consumiendo y proveyendo servicios/recursos directamente entre ellos sin una entidad central siempre activa.
*   d) El modelo P2P es siempre menos seguro que el modelo cliente-servidor.

<details>
  <summary>Ver Respuesta</summary>
  
  **Respuesta correcta: c)**
  
  **Justificación:** La esencia del modelo cliente-servidor es la clara distinción de roles: un servidor siempre activo que ofrece un servicio y clientes que lo consumen. En cambio, en una arquitectura P2P, esta distinción se difumina. Los pares (peers) son nodos con capacidades equivalentes que se conectan directamente entre sí. Un peer que está descargando un archivo (actuando como cliente) puede estar simultáneamente subiendo partes de ese mismo archivo a otros peers (actuando como servidor). Esta dualidad de roles y la descentralización son las características clave del P2P.
</details>
