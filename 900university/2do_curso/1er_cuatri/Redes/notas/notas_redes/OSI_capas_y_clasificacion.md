# Modelo OSI: capas con ejemplos cotidianos

## Capas y ejemplos "tocables"

1. **Física (L1)**
   - Que hace: transporta **bits** como señales electricas, luz o radio.
   - Ejemplo cotidiano: el cable Ethernet, la fibra optica, las ondas Wi-Fi.

2. **Enlace de datos (L2)**
   - Que hace: mueve **tramas** dentro de una red local y usa MAC.
   - Ejemplo cotidiano: un switch casero que conecta PCs y consola; la MAC pegada en la etiqueta de la tarjeta.

3. **Red (L3)**
   - Que hace: enruta **paquetes** entre redes y usa IP.
   - Ejemplo cotidiano: el router que decide por donde sale el trafico hacia Internet.

4. **Transporte (L4)**
   - Que hace: entrega extremo a extremo, controla errores/orden y usa puertos 
	   - (Segmentos : TCP), (Datagramas : UDP).
   - Ejemplo cotidiano: una videollamada estable (TCP para control, UDP para media), o un juego online que prioriza baja latencia.

5. **Sesion (L5)**
   - Que hace: abre, mantiene y cierra sesiones de comunicacion.
	   - "datos"
   - Ejemplo cotidiano: una sesion de login activa en un servicio o una llamada que se establece y se cuelga.

6. **Presentacion (L6)**
   - Que hace: formato, compresion y cifrado de datos.
	   - "datos"
   - Ejemplo cotidiano: una foto en JPEG, un PDF, o HTTPS que cifra lo que ves en el navegador.

7. **Aplicacion (L7)**
   - Que hace: servicios que usa el usuario (protocolos de alto nivel).
	   - "datos"
   - Ejemplo cotidiano: navegar en la web (HTTP), enviar correo (SMTP), resolver nombres (DNS).

• En OSI, el nombre de la información transferida depende de la capa:
  - L1: bits                                             
  - L2: tramas
  - L3: paquetes (datagramas IP) 
  - L4: segmentos (TCP) o datagramas (UDP) 
  - L5–L7: datos/mensajes (no se suele decir “paquetes”)
  
  >Esto no es un capricho sino que cada capa "envuelve" la informacion añadiendo nuevas funcionalidades y dejando intacta las anteriores, por lo que bits es un subconjunto de tramas que es un subconjunto de paquetes que es un subconjunto de segmentos/datagramas que es un subconjunto de datos. Puedes establecer una analogia de forma informal con los numeros. Los naturales estan contenidos en los enteros que estan contenidos en los racionales que estan contenidos por los reales que estan contenidos por los imaginarios, aqui lo mismo. 

## Clasificacion de temas por capa principal

| Tema                                     | Capa principal | Motivo breve                                                       |
| ---------------------------------------- | -------------- | ------------------------------------------------------------------ |
| Tema 1 - Redes de ordenadores e internet | L7             | Vision general de servicios y uso de Internet (toca varias capas). |
| Tema 2 - Introduccion a TCP/IP           | L3             | IP como base del modelo TCP/IP (toca L4 tambien).                  |
| Tema 3 - Tecnologias nivel enlace        | L2             | Tecnologias y mecanismos de enlace.                                |
| Tema 4 - TCP/IP y nivel enlace           | L2             | Enfocado al enlace y su relacion con TCP/IP.                       |
| Tema 5 - IP                              | L3             | Direccionamiento y paquetes IP.                                    |
| Tema 6 - Enrutamiento                    | L3             | Decisiones de encaminamiento entre redes.                          |
| Tema 7 - UDP y TCP                       | L4             | Protocolos de transporte.                                          |
| Tema 8 - Intercambio de datos TCP        | L4             | Detalles del transporte orientado a conexion.                      |
| Tema 9 - Aplicacion                      | L7             | Protocolos y servicios de aplicacion.                              |
| Tema 10 - Virtualizacion                 | L2             | Redes virtuales y switches virtuales (toca otras capas).           |
| Tema 11 - Cloud Computing                | L7             | Servicios ofrecidos al usuario (SaaS/PaaS/IaaS).                   |

