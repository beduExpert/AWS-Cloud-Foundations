## Postwork 

# 1. Objetivo 🎯

- Comprender de que se va a tratar nuestro proyecto que vamos a realizar en las siguiente sesiones.


>💡 **Nota:**
>
>Para identificar que estamos trabajando con nuestro proyecto,encontrarás el siguiente emoji 💻 con la leyenda *Proyecto.*

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

<img src="../assets/arquitectura-Infra.jpg">


En las siguientes sesiones empezaremos a ensuciarnos las manos con nuestro proyecto.


