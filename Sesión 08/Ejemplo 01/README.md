# Ejemplo 1: SMS en registro

## 1. Objetivo 
- Integración de servicios de Messaging, servicios para la comunicación entre servicios de AWS.

![pw-a-donde-vamos-01.png](img/pw-a-donde-vamos-01.png)

## 2. Requisitos
- AWS CLI configurado
- Acceso a AWS consola.
- Un teléfono celular con capacidad para recibir mensajes SMS.

## 3. Desarrollo 

> **💡Nota:**
>
> El siguiente ejemplo y código están destinados únicamente a fines educativos. Asegúrese de personalizarlo, probarlo y revisarlo por su cuenta antes de usar cualquiera de esto en producción.


Hasta este punto ya se tiene un endpoint funcional en el que la aplicación en este caso es "app.edupractice.tk" puede mandar los leads que la página vaya generando a la base de datos.

Como paso siguiente, de cada lead que se registre además de guardarse en base de datos se debe enviar un mensaje SMS a un número celular específico dando aviso del nuevo lead registrado.

Para ello las instancias del cluster deberán tener permiso para poderse comunicar con el servicio de AWS SNS, de la práctica anterior se generó un rol en blanco que no contenía ningún permiso, es momento de agregar un permiso al rol.

1. Solo para cambiar el permiso, ingresar al servicio AWS ECS, dar click al cluster generado.

![pw-ecs-ingress-to-clusters-01.png](img/pw-ecs-ingress-to-clusters-01.png)



2. Acceder a la parte de servicios y dar click en el servicio generado.

![pw-ecs-access-to-service-01.png](img/pw-ecs-access-to-service-01.png)



3. Dar click en la tarea

![pw-ecs-see-task-definition-01.png](img/pw-ecs-see-task-definition-01.png)


4. Dar click en el `Task role`

 ![pw-ecs-access-to-task-role-01.png](img/pw-ecs-access-to-task-role-01.png)


5. Eso abrirá la consola del servicio IAM posicionado directamente en el Rol. Dar clic en "Asociar políticas"

![pw-iam-see-task-execution-role-01.png](img/pw-iam-see-task-execution-role-01.png)

7. a) Buscar los grupos de políticas referentes a SNS, b) seleccionar la política "AmazonSNSFullAccess", c) Click en "Asociar la política".

![pw-iam-add-sns-policy-01.png](img/pw-iam-add-sns-policy-01.png)


8. Listo, con esos pasos se ha dado capacidad para que los contenedores docker tengan acceso al servicio SNS.

![pw-iam-sns-role-added-01.png](img/pw-iam-sns-role-added-01.png)

------------------------------

## Configurar tópico en SNS

Para generar un "Topic" a dónde enviar los mensajes SMS se deben seguir los siguientes pasos:

1. Ir al servicio de AWS SNS:

![pw-goto-sns-aws-01.png](img/pw-goto-sns-aws-01.png)

2. Click en "Temas" (Topic):

![pw-sns-access-to-Topic-01.png](img/pw-sns-access-to-Topic-01.png)

3. Click en "Crear un tema"

![pw-sns-create-topic-02.png](img/pw-sns-create-topic-02.png)

4. a) Seleccionar un tópico Estándar, establecer un nombre descriptivo para el tópico. Proceder con la creación del tópico o tema.

![pw-sns-configure-topic-03.png](img/pw-sns-configure-topic-03.png)

5. El tópico o tema es generado. Click ahora en "Crear una nueva suscripción"

![pw-sns-topic-created-01.png](img/pw-sns-topic-created-01.png)


6. Finalmente a) Seleccionar el protocolo como SMS, b) Establecer el punto de enlace con el número de celular a donde se enviarán las notificaciones, el número debe tener como prefijo el signo +, luego el código del país seguido de los 10 dígitos del número celular. Proceder con la suscripción.

![pw-sns-subscribe-to-a-topic-01.png](img/pw-sns-subscribe-to-a-topic-01.png)


7. La suscripción es generada, dar click en el nombre del Tema:

![pw-sns-suscription-done-access-topic-01.png](img/pw-sns-suscription-done-access-topic-01.png)


8. Copiar el ARN del tópico, será usado en el siguiente paso.

![pw-sns-copy-arn-topic-01.png](img/pw-sns-copy-arn-topic-01.png)


