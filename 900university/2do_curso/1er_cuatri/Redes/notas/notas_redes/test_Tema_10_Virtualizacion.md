# Test de Autoevaluación - Tema 10: Virtualización

Este test contiene preguntas de opción múltiple diseñadas para evaluar la comprensión de los conceptos clave de la virtualización, incluyendo tipos de hipervisores, virtualización de plataforma y los diferentes paradigmas de virtualización.

---

### Pregunta 1

Un administrador de sistemas instala un software de virtualización directamente sobre el hardware "bare-metal" de un servidor físico. Sobre este software, despliega varias máquinas virtuales, cada una con un sistema operativo distinto (Windows Server, Ubuntu, CentOS). ¿Qué tipo de arquitectura de virtualización está utilizando?

*   a) Hipervisor de Tipo II (alojado o hosted).
*   b) Virtualización a nivel de sistema operativo (contenedores).
*   c) Hipervisor de Tipo I (nativo o bare-metal).
*   d) Emulación.

<details>
  <summary>Ver Respuesta</summary>
  
  **Respuesta correcta: c)** 
  
  **Justificación:** Un hipervisor de Tipo I se ejecuta directamente sobre el hardware del anfitrión y es el primer software que se carga. Es, en esencia, un micro-kernel especializado en gestionar el acceso al hardware para las máquinas virtuales que corren sobre él. Ejemplos comunes son VMware vSphere/ESXi, Microsoft Hyper-V y Citrix XenServer. Esta arquitectura se distingue de la de Tipo II, donde el hipervisor es una aplicación que se instala sobre un sistema operativo convencional.
</details>

---

### Pregunta 2

Estás utilizando VirtualBox en tu ordenador personal con Windows 11 para ejecutar una máquina virtual con Ubuntu Desktop. ¿Qué tipo de hipervisor es VirtualBox y cómo se clasifica esta forma de virtualización?

*   a) Hipervisor de Tipo I, porque crea una máquina virtual completa.
*   b) Hipervisor de Tipo II (alojado), porque se instala y se ejecuta como una aplicación más dentro de tu sistema operativo anfitrión (Windows 11).
*   c) Paravirtualización, porque Ubuntu necesita ser modificado para funcionar.
*   d) Virtualización a nivel de kernel, porque se integra directamente en el núcleo de Windows.

<details>
  <summary>Ver Respuesta</summary>
  
  **Respuesta correcta: b)** 
  
  **Justificación:** Los hipervisores de Tipo II, como VirtualBox, VMware Workstation o Parallels, son aplicaciones que se instalan sobre un sistema operativo ya existente (el "host" o anfitrión). Utilizan los servicios del sistema operativo anfitrión para gestionar los recursos de hardware, lo que los hace más sencillos de instalar y usar, siendo la opción predilecta para entornos de escritorio y desarrollo, en contraposición a los de Tipo I que son más comunes en servidores.
</details>

---

### Pregunta 3

La paravirtualización (PV) fue una técnica muy importante antes de que el soporte de hardware para la virtualización (Intel VT-x / AMD-V) se generalizara. ¿Cuál era el requisito fundamental y la principal desventaja de la paravirtualización?

*   a) Requería emular completamente el hardware, lo que la hacía muy lenta.
*   b) Requería que el sistema operativo "invitado" (guest) fuera modificado para realizar "hypercalls" al hipervisor, lo que mejoraba el rendimiento pero sacrificaba la compatibilidad.
*   c) Solo permitía ejecutar un sistema operativo invitado idéntico al del anfitrión.
*   d) No tenía desventajas, era superior en todo a la virtualización completa.

<details>
  <summary>Ver Respuesta</summary>
  
  **Respuesta correcta: b)** 
  
  **Justificación:** A diferencia de la virtualización completa, que engaña al sistema operativo invitado para que crea que se ejecuta sobre hardware real (a menudo con una penalización de rendimiento al traducir instrucciones privilegiadas), la paravirtualización hace que el SO invitado sea "consciente" de que está siendo virtualizado. Para ello, se modifica su kernel para que, en lugar de ejecutar instrucciones de hardware privilegiadas, realice llamadas directas (hypercalls) al hipervisor. Esto es mucho más eficiente, pero tiene la gran desventaja de que requiere modificar el SO invitado, lo cual no siempre es posible (por ejemplo, con Windows).
</details>

---

### Pregunta 4

¿Cuál es la diferencia clave entre la virtualización de plataforma (como en una Máquina Virtual) y la virtualización a nivel de sistema operativo (como en los contenedores Docker o LXC)?

*   a) La virtualización de plataforma es siempre más rápida que los contenedores.
*   b) Las Máquinas Virtuales virtualizan el hardware, permitiendo que cada VM ejecute su propio kernel de sistema operativo completo. Los contenedores, en cambio, virtualizan el sistema operativo, compartiendo todos el mismo kernel del sistema anfitrión.
*   c) Los contenedores pueden ejecutar sistemas operativos totalmente diferentes entre sí (ej. un contenedor Windows y uno Linux) sobre el mismo anfitrión.
*   d) Las Máquinas Virtuales no proporcionan aislamiento, mientras que los contenedores sí.

<details>
  <summary>Ver Respuesta</summary>
  
  **Respuesta correcta: b)**
  
  **Justificación:** Esta es la distinción más importante. Una VM empaqueta una aplicación, sus dependencias Y un sistema operativo completo con su propio kernel. Esto proporciona un aislamiento muy fuerte, pero consume muchos recursos (CPU, RAM, disco). Un contenedor empaqueta una aplicación y sus dependencias, pero se ejecuta como un proceso aislado sobre el kernel del sistema operativo anfitrión. Esto los hace extremadamente ligeros, rápidos de arrancar y eficientes en recursos, pero todos los contenedores en un host deben ser compatibles con el kernel de dicho host (ej. contenedores Linux sobre un host Linux).
</details>

---

### Pregunta 5

Un desarrollador está trabajando con la Java Virtual Machine (JVM). Escribe su código en Java, lo compila a bytecode, y puede ejecutar ese mismo bytecode en Windows, macOS o Linux, siempre que tengan una JVM instalada. ¿Qué tipo de máquina virtual es la JVM?

*   a) Una máquina virtual de sistema o de hardware.
*   b) Una máquina virtual de proceso o aplicación.
*   c) Un hipervisor de Tipo I.
*   d) Un ejemplo de virtualización a nivel de kernel.

<details>
  <summary>Ver Respuesta</summary>
  
  **Respuesta correcta: b)**
  
  **Justificación:** La JVM no simula un ordenador completo (hardware), sino que proporciona un entorno de ejecución abstracto y estandarizado para un solo proceso (la aplicación Java). Su objetivo no es ejecutar un sistema operativo, sino abstraer el sistema operativo y el hardware subyacentes para que el código de la aplicación sea portable. Se inicia cuando se lanza la aplicación y se detiene cuando esta finaliza. Otros ejemplos de este tipo son el Common Language Runtime (.NET) de Microsoft.
</details>
