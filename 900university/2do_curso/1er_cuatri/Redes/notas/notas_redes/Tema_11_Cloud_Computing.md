# Notas Detalladas sobre el Tema 11: Cloud Computing

Este documento expande los conceptos presentados en las diapositivas del "Tema 11", proporcionando explicaciones más profundas sobre los modelos, características y beneficios del Cloud Computing.

---

### **1. ¿Qué es Cloud Computing? La Definición del NIST Desglosada**

La diapositiva 3 presenta la definición del NIST (National Institute of Standards and Technology), que es el estándar de la industria. Aunque es densa, establece que el cloud es un **modelo** que debe cumplir con **5 características esenciales**, ofrecer **3 modelos de servicio** y poder implementarse en **4 modelos de despliegue**.

Vamos a explicarlo de forma sencilla:

Cloud Computing es el acto de **alquilar recursos computacionales (servidores, almacenamiento, bases de datos, software) a través de Internet** en lugar de comprarlos y mantenerlos tú mismo en tu propio centro de datos (*on-premise*).

---

### **2. Las 5 Características Esenciales del Cloud**

Estas son las reglas que un servicio debe cumplir para ser considerado "verdadero cloud".

1.  **Autoservicio Bajo Demanda (On-demand self-service)**
    *   **Qué es:** Un usuario puede provisionar recursos computacionales (como un servidor o una base de datos) de forma automática, sin necesidad de intervención humana por parte del proveedor.
    *   **Ejemplo:** Puedes entrar a la consola web de Amazon Web Services (AWS), hacer unos clics, y tener un servidor virtual funcionando en minutos, pagando con tu tarjeta de crédito. No tienes que llamar a un comercial ni esperar a que un técnico instale algo.

2.  **Acceso Amplio a la Red (Broad network access)**
    *   **Qué es:** Los recursos están disponibles a través de la red (normalmente Internet) y se puede acceder a ellos mediante mecanismos estándar (ej. navegadores web, APIs) desde cualquier tipo de cliente (portátiles, móviles, tabletas).
    *   **Ejemplo:** Puedes gestionar tus servidores en la nube desde tu portátil en la oficina, tu tablet en casa o tu móvil en el aeropuerto.

3.  **Asignación Común de Recursos (Resource pooling) y Multitenancy**
    *   **Qué es:** Los recursos del proveedor (servidores físicos, almacenamiento, etc.) se agrupan para servir a múltiples clientes en un modelo de **multitenancy** (multi-inquilino). Los recursos físicos se asignan y reasignan dinámicamente según la demanda.
    *   **Analogía:** Es como un edificio de apartamentos. Múltiples inquilinos comparten la misma infraestructura subyacente (el edificio, las tuberías), pero cada uno tiene su propio apartamento privado y seguro. No sabes quiénes son tus vecinos ni puedes acceder a sus apartamentos. En la nube, gracias a la virtualización, múltiples clientes (tenants) ejecutan sus aplicaciones en el mismo hardware físico, pero están lógicamente aislados y no pueden acceder a los datos de los demás.

4.  **Rápida Elasticidad (Rapid elasticity)**
    *   **Qué es:** La capacidad de provisionar y liberar recursos de forma rápida y, a menudo, automática, para adaptarse a la demanda. Para el consumidor, los recursos disponibles parecen ser ilimitados.
    *   **Diferencia con Escalabilidad:**
        *   **Escalabilidad:** Es la capacidad de crecer para manejar una carga mayor. Es un plan a largo plazo.
        *   **Elasticidad:** Es la capacidad de crecer *y decrecer* automáticamente en respuesta a picos de demanda. Es una táctica a corto plazo.
    *   **Ejemplo:** Un sitio de comercio electrónico puede tener 10 servidores en un día normal. Durante el Black Friday, el sistema puede **escalar horizontalmente** de forma automática a 100 servidores para manejar el pico de tráfico (*scaling out*). Cuando el pico pasa, automáticamente vuelve a 10 servidores (*scaling in*), dejando de pagar por los 90 que ya no necesita.

5.  **Servicio Medido (Measured service)**
    *   **Qué es:** El uso de los recursos se monitoriza, se controla y se reporta de forma transparente tanto para el proveedor como para el cliente. Se paga solo por lo que se consume (pago por uso, *pay-as-you-go*).
    *   **Ejemplo:** AWS te cobrará por el número de horas que tu servidor estuvo encendido, los gigabytes de almacenamiento que ocupaste y los datos que transferiste. Si apagas el servidor, dejas de pagar por él.

---

### **3. Los 3 Modelos de Servicio: IaaS, PaaS y SaaS (Pizza as a Service)**

Estos modelos definen qué parte de la "pila tecnológica" gestionas tú y qué parte gestiona el proveedor de la nube. La analogía de la "Pizza como Servicio" lo explica perfectamente.

![Pizza as a Service](https://i.imgur.com/35mgy32.png)

#### **On-Premise (La Pizza Hecha en Casa)**
Tú gestionas absolutamente todo. Compras el horno, el gas, la harina, el tomate... Haces la masa, la cocinas y la sirves. Es el máximo control, pero también la máxima responsabilidad y coste inicial.

---

#### **IaaS (Infrastructure as a Service)**
*   **Qué es:** El proveedor de la nube te alquila los "bloques de construcción" fundamentales: servidores virtuales (VMs), almacenamiento y redes.
*   **Tú Gestionas:** El sistema operativo, las librerías, las bases de datos y tu aplicación.
*   **El Proveedor Gestiona:** El hardware físico (servidores, redes, almacenamiento) y la capa de virtualización.
*   **Analogía (Pizza para llevar y hornear):** La pizzería te da la masa, la salsa y los ingredientes. Tú usas tu propio horno y tu propia mesa para cocinarla y comerla.
*   **Ejemplos:** **Amazon EC2** (servidores virtuales), **Amazon S3** (almacenamiento), **Google Compute Engine**, **Azure VMs**.

#### **PaaS (Platform as a Service)**
*   **Qué es:** El proveedor te ofrece una plataforma completa para desarrollar, desplegar y gestionar aplicaciones, sin que tengas que preocuparte por la infraestructura subyacente.
*   **Tú Gestionas:** Tu aplicación (el código) y sus datos.
*   **El Proveedor Gestiona:** Todo lo demás: SO, parches de seguridad, bases de datos, servidores web, etc.
*   **Analogía (Pizza a domicilio):** La pizzería te entrega la pizza ya cocinada. Tú solo tienes que poner la mesa y los refrescos. No te preocupas del horno ni de los ingredientes.
*   **Ejemplos:** **Heroku**, **Google App Engine**, **AWS Elastic Beanstalk**, **Red Hat OpenShift**.

#### **SaaS (Software as a Service)**
*   **Qué es:** Software listo para usar que consumes directamente desde Internet, normalmente a través de un navegador web.
*   **Tú Gestionas:** Prácticamente nada, solo tu cuenta de usuario y tus datos dentro de la aplicación.
*   **El Proveedor Gestiona:** Toda la pila tecnológica de principio a fin.
*   **Analogía (Cenar en la pizzería):** Vas al restaurante. Ellos lo hacen todo: cocinan, sirven, te dan mesa y bebida. Tú solo disfrutas de la pizza.
*   **Ejemplos:** **Gmail**, **Office 365**, **Salesforce**, **Dropbox**.

**Tabla de Responsabilidades:**

| | On-Premise | IaaS | PaaS | SaaS |
| :--- | :---: | :---: | :---: | :---: |
| **Aplicación** | **Tú** | **Tú** | **Tú** | Proveedor |
| **Datos** | **Tú** | **Tú** | **Tú** | Proveedor |
| **Runtime/Middleware**| **Tú** | **Tú** | Proveedor | Proveedor |
| **Sistema Operativo**| **Tú** | **Tú** | Proveedor | Proveedor |
| **Virtualización** | **Tú** | Proveedor | Proveedor | Proveedor |
| **Servidores** | **Tú** | Proveedor | Proveedor | Proveedor |
| **Almacenamiento** | **Tú** | Proveedor | Proveedor | Proveedor |
| **Red** | **Tú** | Proveedor | Proveedor | Proveedor |

---

### **4. Los 4 Modelos de Despliegue**

Estos modelos describen dónde reside la infraestructura de la nube y quién la posee.

1.  **Nube Pública (Public Cloud)**
    *   **Qué es:** La infraestructura pertenece y es gestionada por un proveedor tercero (como AWS, Google, Microsoft) y los servicios se ofrecen al público general a través de Internet. Es un entorno multi-inquilino.
    *   **Ventajas:** Máximo ahorro (sin inversión inicial), elasticidad casi infinita, no hay mantenimiento.
    *   **Desventajas:** Menor control sobre la infraestructura y la seguridad (el "vecindario ruidoso" puede ser un problema), posibles preocupaciones sobre la soberanía de los datos (¿dónde están físicamente mis datos?).

2.  **Nube Privada (Private Cloud)**
    *   **Qué es:** La infraestructura de la nube es utilizada por una única organización. No se comparte con nadie. Puede estar alojada en el propio centro de datos de la organización (*on-premise*) o ser gestionada por un tercero (*hosted private cloud*).
    *   **Ventajas:** Máximo control sobre la seguridad y los datos, ideal para cumplir con regulaciones estrictas (ej. sector financiero, salud).
    *   **Desventajas:** Es la opción más cara, la organización es responsable de comprar y mantener la infraestructura, no ofrece la misma elasticidad que la nube pública.

3.  **Nube Híbrida (Hybrid Cloud)**
    *   **Qué es:** Es una combinación de una nube pública y una nube privada, que operan juntas.
    *   **Funcionamiento:** Las organizaciones suelen mantener sus datos más críticos y sensibles en su nube privada por seguridad y control, pero utilizan la nube pública para cargas de trabajo menos sensibles o para aprovechar su elasticidad.
    *   **Ejemplo:** Una empresa de retail utiliza su nube privada para la base de datos de clientes, pero durante una campaña de marketing, despliega servidores web adicionales en la nube pública para manejar el pico de visitas (esto se conoce como *cloud bursting*).

4.  **Nube Comunitaria (Community Cloud)**
    *   **Qué es:** Un modelo menos común donde la infraestructura es compartida por varias organizaciones que tienen un interés común (ej. mismos requisitos de seguridad, misma misión o sector).
    *   **Ejemplo:** Varias universidades podrían crear una nube comunitaria para compartir recursos de computación de alto rendimiento para proyectos de investigación.
