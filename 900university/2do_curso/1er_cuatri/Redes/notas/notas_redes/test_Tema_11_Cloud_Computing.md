# Test de Autoevaluación - Tema 11: Cloud Computing

Este test contiene preguntas de opción múltiple diseñadas para evaluar la comprensión de los modelos de servicio y despliegue de Cloud Computing, así como sus características y beneficios principales.

---

### Pregunta 1

Una startup de desarrollo de software quiere lanzar su nueva aplicación web. No tienen capital para comprar servidores y no quieren preocuparse por mantener el sistema operativo, el servidor web o la base de datos. Solo quieren subir su código (escrito en Python) y que la plataforma se encargue de ejecutarlo y escalarlo automáticamente. ¿Qué modelo de servicio en la nube se adapta mejor a sus necesidades?

*   a) Infraestructura como Servicio (IaaS).
*   b) Plataforma como Servicio (PaaS).
*   c) Software como Servicio (SaaS).
*   d) Nube Privada.

<details>
  <summary>Ver Respuesta</summary>
  
  **Respuesta correcta: b)** 
  
  **Justificación:** El modelo PaaS (Platform-as-a-Service) proporciona una plataforma y un entorno que permite a los desarrolladores construir aplicaciones y servicios sobre ella. Abstrae toda la infraestructura subyacente (servidores, S.O., redes), permitiendo que el equipo se centre exclusivamente en el código y la lógica de la aplicación. Ejemplos de esto son Heroku, Google App Engine o AWS Elastic Beanstalk. En IaaS tendrían que gestionar las VMs y el S.O., y en SaaS simplemente usarían una aplicación ya construida.
</details>

---

### Pregunta 2

Contratas un servicio de CRM (Customer Relationship Management) como Salesforce. Accedes a la aplicación a través de tu navegador web, creas usuarios para tu equipo de ventas y personalizas algunos campos, pero no tienes ningún control sobre la infraestructura subyacente ni sobre las actualizaciones del software. ¿Qué modelo de servicio en la nube estás utilizando?

*   a) Infraestructura como Servicio (IaaS).
*   b) Plataforma como Servicio (PaaS).
*   c) Software como Servicio (SaaS).
*   d) Nube Comunitaria.

<details>
  <summary>Ver Respuesta</summary>
  
  **Respuesta correcta: c)** 
  
  **Justificación:** SaaS (Software-as-a-Service) es un modelo donde se ofrece una aplicación de software completa, lista para usar, a través de Internet. El usuario final simplemente consume el servicio, generalmente a través de un navegador web, sin preocuparse por la instalación, mantenimiento o gestión de la infraestructura. Gmail, Office 365 y Salesforce son ejemplos clásicos de SaaS.
</details>

---

### Pregunta 3

Una de las cinco características esenciales del Cloud Computing según el NIST es la "rápida elasticidad" (rapid elasticity). ¿A qué se refiere este concepto?

*   a) A la capacidad de la red para estirarse físicamente y cubrir más distancia.
*   b) A la capacidad de provisionar y liberar recursos de computación (como CPU, RAM o VMs) de forma automática y rápida para adaptarse a la demanda, escalando hacia arriba o hacia abajo según sea necesario.
*   c) A que los datos se almacenan en bandas de goma para que ocupen menos espacio.
*   d) A la habilidad de acceder a los servicios desde cualquier ubicación geográfica.

<details>
  <summary>Ver Respuesta</summary>
  
  **Respuesta correcta: b)** 
  
  **Justificación:** La elasticidad es uno de los mayores atractivos del cloud. Permite a las organizaciones manejar picos de demanda (ej. una tienda online durante el Black Friday) provisionando más recursos de forma automática (escalado horizontal o "scaling out") y, de la misma forma, liberar esos recursos cuando la demanda baja para no pagar por ellos. Desde la perspectiva del consumidor, los recursos disponibles parecen ser ilimitados y se pueden ajustar en cualquier momento.
</details>

---

### Pregunta 4

Un gran banco decide modernizar su infraestructura de TI. Por motivos de seguridad y regulación, necesita mantener el control total sobre el hardware y la ubicación de los datos de sus clientes. Construye su propio centro de datos utilizando tecnologías de cloud, para uso exclusivo de sus propios departamentos. ¿Qué modelo de despliegue de nube ha implementado el banco?

*   a) Nube Pública.
*   b) Nube Híbrida.
*   c) Nube Comunitaria.
*   d) Nube Privada.

<details>
  <summary>Ver Respuesta</summary>
  
  **Respuesta correcta: d)**
  
  **Justificación:** Un modelo de nube privada implica que la infraestructura de cloud es operada y utilizada exclusivamente por una única organización. La organización puede gestionarla ella misma o contratar a un tercero para que lo haga, pero los recursos no se comparten con el público general. Este modelo se elige cuando el control, la seguridad y el cumplimiento normativo son las máximas prioridades.
</details>

---

### Pregunta 5

Una empresa utiliza una nube privada para alojar su base de datos de clientes (datos sensibles), pero para gestionar los picos de tráfico de su página web pública (datos no sensibles), utiliza recursos de computación de un proveedor de nube pública como AWS. Ambos entornos están interconectados. ¿Qué modelo de despliegue describe esta arquitectura?

*   a) Nube Pública.
*   b) Nube Híbrida.
*   c) Nube Comunitaria.
*   d) Nube Privada.

<details>
  <summary>Ver Respuesta</summary>
  
  **Respuesta correcta: b)**
  
  **Justificación:** Un modelo de nube híbrida es la composición de dos o más modelos de despliegue distintos (privado, comunitario o público) que permanecen como entidades únicas pero están unidas por tecnología estandarizada o propietaria que permite la portabilidad de datos y aplicaciones entre ellas. El caso de uso más común es, precisamente, usar una nube privada para cargas de trabajo críticas o sensibles y una nube pública para cargas de trabajo menos sensibles o con alta variabilidad, en una estrategia conocida como "cloud bursting".
</details>
