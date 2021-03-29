# AWS Cloud Foundations

## :wave: Bienvenidos

Este curso sirve como preparación para la certificación [AWS Cloud Practitioner (CLF-C01)](https://aws.amazon.com/es/certification/certified-cloud-practitioner/) que valida los conocimientos del ecosistema AWS a nivel practicante, es decir, serás capaz de diseñar y proponer soluciones centradas en los servicios e infraestructura que ofrece AWS en la nube con la finalidad de soportar el **desarrollo**, **despliegue** y **consumo de aplicaciones web**.

## :dart: Objetivo

Fungir como una introducción a los principales conceptos del cómputo en la nube desde un enfoque tanto de gestión y negocio como práctico. Con los conocimientos adquiridos el podrás armar casos de negocios que consideren los beneficios de diferentes tipos y niveles de servicio que ofrece **AWS** frente a otras alternativas (**Google**, **IBM**, **Microsoft**, etc).

Entenderás los **economics** y herramientas que te permitirán **optimizar los costos** asociados servicios e infraestructura en la plataforma de AWS que se ajusten a las necesidades de cada proyecto o negocio.

Podrás interactuar con los servicios Core de AWS tanto de cómputo **(EC2)**, **redes** y **distribución de contenido**, **almacenamiento**, **integración de aplicaciones**, **administración** y **gobernanza**.

Pondrás en práctica conceptos fundamentales para mantener una arquitectura confiables y segura, tales como: **Framework AWS Well Architectured**, el **modelo de responsabilidad compartida** y las **políticas de uso**. Además de las funciones para administración de usuarios, permisos y autenticación, así como las mejores prácticas sobre tolerancia a fallos, alta disponibilidad y recuperación ante desastres. 

## :gear: Requerimientos

No es necesario, pero sí recomendable tener **conocimientos previos de programación** y experiencia en el uso del **Sistema Operativo Linux**, así como estar familiarizado con los siguientes temas:

+ Redes y sistemas
+ Bases de datos SQL
+ Protocolos de red
+ TCP/IP
+ HTTP

# 💻 Proyecto

El proyecto contempla el uso de varios servicios de AWS con el fin de adquirir la visibilidad como todos ellos operan en conjunto.
Se simulará un formulario de contacto de clientes o captador de clientes (leads), al momento de que el usuario final llene los campos y de click en el botón de envío se enviarán los datos a un balanceador de carga, el balanceador de carga con su certificado SSL reenviará los datos a alguna de las intancias que estén ejecutando el código que se encargará de tomar la información y guardarla en base de datos además de despachar la información necesaria para dar aviso a un número celular en cuanto un nuevo usuario deje sus datos de contacto.

En general consistirá en las siguientes partes:

- Una interfaz hosteada en **S3** con **HTML**, **CCS** y **Javascript**, esta será la parte de cara al usuario final.
- Un balanceador de carga con su **certificado SSL** para que la información viaje segura.
- Un par de **instancias EC2** de AWS, el código se ejecutará en contenedores Docker.
- Se tendrá un **servicio SMS** listo para el envío de mensajes al tener un nuevo cliente.

Se recomienda encarecidamente que todo el proyecto sea generado en una misma unidad regional, en este curso se estará usando la región `us-east-1`.

Toda esta infraestructura debe tener un **certificado de seguridad** para operar, será usado AWS Certificate Manager para generarlo, por lo que sería necesario configurar **Route53** para que un dominio sea resuelto. 

<img src="/assets/arquitectura-Infra.jpg">


## :bookmark_tabs: Temario
- [`Sesión 01:` Fundamentos de Cómputo en la Nube](https://github.com/beduExpert/AWS-Cloud-Foundations2020/tree/main/Sesi%C3%B3n%2001)

- [`Sesión 02:` Infraestructura y servicios en la nube](https://github.com/beduExpert/AWS-Cloud-Foundations2020/tree/main/Sesi%C3%B3n%2002)

- [`Sesión 03:` Contenido y redes en AWS](https://github.com/beduExpert/AWS-Cloud-Foundations2020/tree/main/Sesi%C3%B3n%2003)

- [`Sesión 04:` Seguridad y compliance](https://github.com/beduExpert/AWS-Cloud-Foundations2020/tree/main/Sesi%C3%B3n%2004)

- [`Sesión 05:` Almacenamiento con AWS](https://github.com/beduExpert/AWS-Cloud-Foundations2020/tree/main/Sesi%C3%B3n%2005)

- [`Sesión 06:` Cómputo en la nube con AWS](https://github.com/beduExpert/AWS-Cloud-Foundations2020/tree/main/Sesi%C3%B3n%2006)

- [`Sesión 07:` Arquitectura AWS](https://github.com/beduExpert/AWS-Cloud-Foundations2020/tree/main/Sesi%C3%B3n%2007)

- [`Sesión 08:` Integraciones y gobernanza](https://github.com/beduExpert/AWS-Cloud-Foundations2020/tree/main/Sesi%C3%B3n%2008)

