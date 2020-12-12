# Postwork


## 1. Objetivo 

- Aprender como hacer despliegue automático de un aplicación web (la misma que ya se había trabajado anteriormente) en instancias que escalan automáticamente, la aplicación se ejecutará en una instancia de Docker. La idea es que además el despliegue sea orquestado de manera automática en instancias generadas también de manera automática, dicha generación de instancias estará a cargo del servicio `Elastic Comtainer Service`

## 2. Requisitos 
- Acceso a AWS Console (WEB)
- Comprender como funciona un balanceador de carga
- Conocimientos básicos de docker
- Cliente de git para manejo de código en repositorios de git

## 3. Desarrollo 

> **💡Nota:**
>
> El siguiente ejemplo y código están destinados únicamente a fines educativos. Asegúrese de personalizarlo, probarlo y revisarlo por su cuenta antes de usar cualquiera de esto en producción.


Anteriormente ya se tenía trabajando un balanceador de carga, el balanceador redirigía el tráfico hacia instancias EC2, en ellas se tenía Docker ejecutándose. Si se requería hacer un nuevo despliegue con una nueva versión se requiere regenerar la imagen, entrar a las instancias EC2 y manualmente ejecutar el comando `docker pull` para actualizar la imagen con la nueva versión del software. 


## Acceso a IAM para roles del cluster

Se debe establecer un rol de tarea de ejecución que será usado posteriormente al querer configurar una `task definition` en el servicio ECS

Para ello:

1. Ir al servicio IAM

![pw-access-to-iam--web-console-01.png](img/pw-access-to-iam--web-console-01.png)

2. Ir a la parte de roles.

![pw-roles-main-menu-01.png](img/pw-roles-main-menu-01.png)


3. Dar click en "Crear un rol"

![pw-iam-create-role-button-01.png](img/pw-iam-create-role-button-01.png)

4. Seleccionar "Servicio de AWS", después seleccionar "Elastic container service", en el apartado inferior seleccionar "Elastic container Service Task", después click en "Sigueinte:Permisos"
![pw-iam-select-role-for-ec2-01.png](img/pw-iam-select-role-for-ec2-01.png)

5. Normalmente se deberían seleccionar los permisos para el rol, por lo pronto se dejará sin seleccionar ninguna política.
![pw-iam-select-sqs-services-01.png](img/pw-iam-select-sqs-services-01.png)

6. Añadir una etiqueta

![pw-iam-assign-tags-to-role-01.png](img/pw-iam-assign-tags-to-role-01.png)

7. Establecer un nombre descriptivo para el rol.

![pw-sqs-assign-name-and-description-01.png](img/pw-sqs-assign-name-and-description-01.png)

8. Finalmente se tiene el rol creado.
![pw-iam-role-created-01.png](img/pw-iam-role-created-01.png)


## Grupos de seguridad.
Al momento se tienen grupos de seguridad para conexión a base de datos, conexión por ssh, conexión web, se requerirá un nuevo grupo de seguridad para permitir la redirección de tráfico desde el balanceador de carga a instancias EC2 por puertos dinámicos. Los puertos dinámicos son los que darán la capacidad de desplegar una imagen Docker sin comprometer una imagen ya en ejecución mientras la nueva imagen es desplegada.
Para ello:

1. Ingresar al servicio VPC
![pw-vpc-menu-access-01.png](img/pw-vpc-menu-access-01.png)

2. Dirigirse a la sección de Grupos de seguridad
![pw-vpc-access-security-groups-01.png](img/pw-vpc-access-security-groups-01.png)

3. Crear un nuevo grupo de seguridad
![pw-vpc-create-new-security-group-01.png](img/pw-vpc-create-new-security-group-01.png)


4. Establecer como parámetros:
a) Un nombre para el grupo de seguridad, para este ejemplo `seg-gr-dynamic-01`
b) Establecer una descripción.
c) Establecer la VPC donde funcionará el grupo de seguridad, establecer la VPC con la que se ha venido desarrollando el proyecto.
d) Establecer un rango de puertos 49153 - 65535 con permisos de ser usados desde cualquier dirección IP (Anywhere), notar que el tipo es "Custom TPC"
e) Establecer un rango de puertos 32768 - 61000 con permisos de ser usados desde cualquier dirección IP (Anywhere), notar que el tipo es "Custom TPC"
f) Como regla de salida se puede dejar la que viene por defecto.

![pw-vpc-new-security-group-01.png](img/pw-vpc-new-security-group-01.png)

5. Con estos pasos ya se tiene el grupo de seguridad que será usado para las instancias EC2 que serán creadas por el servicio Elastic Container Service.
![pw-vpc-security-group-done-01.png](img/pw-vpc-security-group-done-01.png)


1. Antes de ejecutar el pipeline se deberá crear un registro para almacenar la imagen de docker que vaya resultando de la etapa de "Build", 
para ello hay que ir al servicio "Elastic Container Registry" 


![pw-ecr-search-service-01.png](img/pw-ecr-search-service-01.png)


2. Click en **Get Started**

![pw-ecr-get-sterted-01.png](img/pw-ecr-get-sterted-01.png)


3. a) Establecer la visibilidad del registro, se establecerá como privado, b) Seleccionar un nombre para el repositorio. 

![pw-ecr-create-repository-01.png](img/pw-ecr-create-repository-01.png)

El repositorio es creado:

![pw-ecr-repository-created-01.png](img/pw-ecr-repository-created-01.png)

Copiar el "Repository name", será usado en la siguiente etapa, para este ejemplo el nombre es `generador-leads`
Copiar la "URI" del repositorio generado, dicha URI será usada al configurar una `task definition` en el servicio ECS más adelante.

Para configurar la etapa de Build se deben seguir los siguientes pasos:

1. Buscar el servicio "Code Build" e ingresar a él.

![pw-cb.access-01.png](img/pw-cb.access-01.png)



2. Click en "Crear el proyecto de compilación"

![pw-cb-create-compilation-project-01.png](img/pw-cb-create-compilation-project-01.png)


3. Al ingresar a la configuración se deberá establecer los siguientes datos:
a) Establecer un nombre para el proyecto de compilación.
b) Establecer de donde saldrá el código a ser compilado, para ello se seleccionará el servicio "CodeCommit"
c) Se seleccionará el repositorio git.

![pw-code-build-generate-task-01.png](img/pw-code-build-generate-task-01.png)

4. Seleccionar la rama de la que se deberá extraer el código fuente.

![pw-code-build-branches-origin-code-01.png](img/pw-code-build-branches-origin-code-01.png)


5. Para configurar el entorno donde se construirá la imagen se deberá especificar:
a) Seleccionar imagen administrada
b) Seleccionar el sistema operativo Amazon Linux 2
c) Seleccionar "Standard"
d) Seleccionar la imagen más reciente
e) Seleccionar la imagen mas reciente de la imagen
f) Seleccionar tipo de entorno "Linux"
g) Habilitar el modo "privilegiado" ya que se generará una imagen de Docker.

![pw-code-build-configure-environment-01.png](img/pw-code-build-configure-environment-01.png)


a) Seleccionar un nuevo rol, el rol es necesario para acceder al repositorio git con el código fuente.
b) Establecer un nombre descriptivo para el rol.
c) El tiempo de espera se debe establecer en 10 minutos
d) El tiempo de espera en cola se deberá establecer en 15 minutos
e) No se debe instalar ningún certificado.

![pw-code-build-environ-config-02.png](img/pw-code-build-environ-config-02.png)


5. a) Establecer la VPC en la cual se conectará la instancia de Code buil para generar la imagen, establecer la VPC con la que se ha trabajado el proyecto.
b) Seleccionar subredes privadas que tengan el acceso por NAT Gateway hacia internet.
c) Seleccionar un grupo de seguridad con acceso hacia internet, los grupos que se han manejado hasta ahora no tienen restricción en tráfico de salida.
d) Dar click en "Validar la configuración de la VPC", e) Se deberá mostrar un mensaje confirmando el acceso a internet.

![pw-code-build-environ-config-03.png](img/pw-code-build-environ-config-03.png)

6. Se debe especificar el tamaño de la instancia donde se generara la imagen de docker
a) El programa no es nada pesado, se puede especificar el tamaño de la instancia en la más pequeña
b) Establecer la variable de entorno `AWS_DEFAULT_REGION` con el valor `us-east-1`
c) Establecer la variable de entorno `AWS_ACCOUNT_ID` con el valor  del número de cuenta, dicho valor puede ser consultado en el menú superior derecho.
d) Establecer la variable de entorno `IMAGE_TAG` con el valor `latest`
e) Establecer la variable de entorno `IMAGE_REPO_NAME` con el nombre del repositorio de imágenes docker recién creado.

![pw-code-build-instance-size-and-env-variables-01.png](img/pw-code-build-instance-size-and-env-variables-01.png)

7. EL archivo de especificación es un archivo que viene en la raiz del proyecto, se llama `buildspec.yml`, en él se definen los pasos a seguir para generar la imagen docker.

![pw-code-build-compilation-settings-01.png](img/pw-code-build-compilation-settings-01.png)

8. a) Especificar una construcción sin artefactos, b),c) Deshabilitar los logs. Finalizar dando click en "Crear el proyecto de compilación"

![pw-code-build-last-step-01.png](img/pw-code-build-last-step-01.png)

Después de un minuto el proyecto de compilación se genera.

![pw-code-build-compilation-rpoyect-created-01.png](img/pw-code-build-compilation-rpoyect-created-01.png)



Antes de finalizar, habrá que ir al servicio IAM a agregar un rol a la política para asegurar que la imagen resultante se puede subir al registro en el servicio ECR, para ello:

a) Buscar en los roles, el rol recién establecido en "Code Build" (en este caso role-code-build-leads-generator-01)
b) a dicho rol en la sección de "permisos"
c) Agregar la política "AmazonEC2ContainerRegistryPowerUser"
d) Una vez agregada la política se verá como ha sido agregada correctamente.

![pw-iam-add-powe-user-to-ecr-01.png](img/pw-iam-add-powe-user-to-ecr-01.png)

Para verificar que funciona correctamente se deberá seleccionar el proyecto de compilación recién creado y dar click en "Iniciar la compilación"

![pw-code-build-start-compilation-01.png](img/pw-code-build-start-compilation-01.png)

Se Abrirá una ventana con los datos para iniciar el proceso, basta con simplemente "Iniciar la compilación"

![pw-code-build-start-compilation-02.png](img/pw-code-build-start-compilation-02.png)


Se iniciará el trabajo de compilación 

![pw-code-build-compilation-stage-started-01.png](img/pw-code-build-compilation-stage-started-01.png)


Después de un par de minutos se confirma que el trabajo fue creado satisfactoriamente

![pw-code-build-job-done-01.png](img/pw-code-build-job-done-01.png)



------------------------------------------

El paso siguiente es definir dónde se ejecutará la imagen de docker generada por AWS CodeBuild.

1. Para ello se buscará el servicio ECS (Elastic Container Service)  y se ingresará en él.

![pw-ecs-acces-from-admin-web-01.png](img/pw-ecs-acces-from-admin-web-01.png)


2. Dar click en la sección "Clusters"

![pw-ecs-container-service-principal-menu-01.png](img/pw-ecs-container-service-principal-menu-01.png)

3. Dar click en "Create Cluster"

![pw-ecs-create-cluster-specific-menu-01.png](img/pw-ecs-create-cluster-specific-menu-01.png)

4. Seleccionar EC2 Linux + Networking

![pw-ecs-select-ec2-and-network-01.png](img/pw-ecs-select-ec2-and-network-01.png)


5. a) Establecer un nombre al cluster
b) Establecer el tamaño de instancia donde se ejecutará la imagen de docker, en este caso t2.medium
c) Establecer el número de instancias en 2
d) Establecer la imagen del sistema operativo del Host, en este caso Amazon Linux 2 AMI
e) Establecer el volumen de disco duro, el mínimo son 30 GB, es suficiente para el ejercicio.
f) No se requerirá acceder a las instancias que ECS genere, no seleccionar ninguna llave.

![pw-ecs-create-cluster-set-ec2-size-01.png](img/pw-ecs-create-cluster-set-ec2-size-01.png)


6. Seleccionar la red VPC donde se establecerán las instancias (seleccionar la VPC que se ha trabajado en todo el proyecto), establecer las subredes públicas donde residirán las instancias creadas, se establece que se debe asignar una IP pública y se debe seleccionar el grupo de seguridad recién configurado diseñado para puertos dinámicos.

![pw-ecs-set-networking-01.png](img/pw-ecs-set-networking-01.png)

7. Establecer en el punto a) un nuevo rol que ECS creará automáticamente, b) asignar una etiqueta solo para identificación.

![pw-ecs-set-role-and-tags-01.png](img/pw-ecs-set-role-and-tags-01.png)

En ese momento el cluster comienza a generarse.

![pw-ecs-cluster-creating-01.png](img/pw-ecs-cluster-creating-01.png)

Al dar click en "View Cluster" se podrá ver el custer generado... después de aproximadamente 5 minutos se verán nuevas instancias EC2 generadas automáticamente, en estas instancias es donde se ejecutará la imagen docker creada por AWS Code Build.

![pw-ecs-cluster-instances-created-done-01.png](img/pw-ecs-cluster-instances-created-done-01.png)

Una vez generado el cluster, se debe configurar una "tarea",  en una tarea se definen las reglas bajo las cuales el o los contenedores operarán, se puede ver como un template o plantilla, más tarde se asociará esta plantilla con el cluster de instancias EC2 recién generado por medio de un _servicio_.


1. a) Ir a "Task definitions", b) después dar click en "Create new Task Definition"

![pw-ecs-tas-definition-01.png](img/pw-ecs-tas-definition-01.png)

2. Seleccionar una tarea compatible con instancias EC2.

![pw-ecs-select-ec2-type-task-01.png](img/pw-ecs-select-ec2-type-task-01.png)

3. Configurar la tarea
a) Establecer un nombre de la tarea descriptivo.
b) Establecer el rol que permitirá a los contenedores comunicarse con otros servicios de AWS. Se generó con anterioridad.
c) El modo de red debe quedar como "Bridge"

![pw-ecs-configure-task-01.png](img/pw-ecs-configure-task-01.png)

a) Establecer el Rol de ejecución como "create new role"
b) Establecer el tamaño de la memoria tarea de ejecución en 256 MiB
c) Establecer el tamaño de CPU de la tarea de ejecución en 1024 unidades

![pw-ecs-configure-task-02.png](img/pw-ecs-configure-task-02.png)

a) Se procederá a configurar el contenedor que será ejecutado en esta tarea

![pw-ecs-configure-task-03.png](img/pw-ecs-configure-task-03.png)



a) Establecer el nombre del contenedor como `generados-leads-01`, es importante establecerlo así ya que el código fuente hace referencia a este nombre de contenedor en el archivo buildspec.yml
b) Establecer la URI de la imagen que se generó al configurar el repositorio en ECR, a la URI habrá que agregarle al final la etiqueta de la imagen de docker `:latest`
c) Establecer un limite de memoria hard de 256 MiB
d) Establecer un mapeo dinámico de puertos poniendo como _Host port_ el puerto 0, el puerto _Container port_ deberá ser establecido en 8000, 8000 es el puerto donde la aplicación se ejecuta.

![pw-ecs-task-config-container-01.png](img/pw-ecs-task-config-container-01.png)

El contenedor de docker como se se ha comprobado en ejercicios anteriores requiere algunas variables de entorno para poder ser ejecutado.
a) Se debe establecer la variable `DB_HOST`  con el valor de la instancia RDS de base de datos.
b)  Se debe establecer la variable `DB_NAME`  con el valor especificado al generar la instancia de base de datos RDS.
c)  Se debe establecer la variable `DB_USER`  con el valor  especificado al generar la instancia de base de datos RDS.
d)  Se debe establecer la variable `REGION`  con el valor de la región donde se esté trabajando y configurando toda la aplicación.
e)  Se debe establecer la variable `DB_PASSWORD`  con el valor  especificado al generar la instancia de base de datos RDS.

![pw-ecs-task-config-container-02.png](img/pw-ecs-task-config-container-02.png)


Los otros parámetros del contenedor se pueden dejar por defecto. Click en "Add"

![pw-ecs-task-config-container-05.png](img/pw-ecs-task-config-container-05.png)


Los parámetros de la tarea de ejecución que siguen para configurar deben quedar como se aprecia en la imagen.
 Click en "Create"

![pw-ecs-task-configure-task-done-01.png](img/pw-ecs-task-configure-task-done-01.png)


-------------------------
Sigue Generar servicio en el cluster, el servicio se encargará de tomar la tarea recién definida y ejecutarla en las instancias EC2 que ha desplegado el cluster.

1. Ir al cluster que se ha generado con anterioridad

![pw-ecs-configure-service-01.png](img/pw-ecs-configure-service-01.png)


2. a) Seleccionar "Services", b) Click en el botón "Create"

![pw-ecs-configure-service-02.png](img/pw-ecs-configure-service-02.png)


3. Para configurar el servicio se debe establecer los siguientes valores.
a) El servicio será de tipo "EC2"
b) Seleccionar la `task definition` recién creada, c) con la revisión que tenga la palabra (latest)
d) Seleccionar el nombre del cluster, seleccionar el cluster recien creado
e) Establecer un nombre descriptivo para el servicio, en este caso será `servicio-generador-leads-01`
f) En tipo de servicio establecer "REPLICA"
g) En el número de tareas a ejecutar establecer 2.
h) Establecer el mínimo de instancias saludables al 50%
i) Establecer el máximo de instancias saludables al 100%

![pw-ecs-configure-service-03.png](img/pw-ecs-configure-service-03.png)


a) En el tipo de despliegue establecer "Rolling update"
b) En Task Placement establecer "One Task Per Host"
Dar clicn en "Next step"

![pw-ecs-configure-service-04.png](img/pw-ecs-configure-service-04.png)


a) Establecer el tipo de balanceador de carga como Application Load Balancer (anteriormente se había generado uno en el módulo "Cómputo en la nube")
b) Seleccionar "Create new role"
c) Seleccionar el balanceador de carga que se encargará de redirigir el tráfico.
d) Click en "Add to load balancer"

![pw-ecs-config-network-02.png](img/pw-ecs-config-network-02.png)

Como paso siguiente seleccionar el "Target Group" asociado al balanceador de carga, luego click en "Next step"

![pw-ecs-task-config-network-04.png](img/pw-ecs-task-config-network-04.png)

Se puede dejar en blanco las políticas de Auto Scaling, click en "Next Step"

![pw-ecs-config-network-06.png](img/pw-ecs-config-network-06.png)


Al ver el resumen de todo lo que se configurará y estar confirme se puede dar click en "Create Service"

![pw-ecs-configure-task-done-01.png](img/pw-ecs-configure-task-done-01.png)


Hay que esperar a que el servicio sea generado tarda pocos segundos, se puede dar click en "View Service" al finalizar. 

![pw-ecs-task-building-01.png](img/pw-ecs-task-building-01.png)



Al dirigirse al servicio "EC2" a la sección de "Target Groups", se podrá apreciar como el servicio ejecutándose ECS ha asociado correctamente las instancias del cluster con el balanceador de carga.

![pw-ec2-target-group-instances-asociated-01.png](img/pw-ec2-target-group-instances-asociated-01.png)

Pasados algunos minutos se podrá comprobar al ingresar al la URL del balanceador de carga en el path de la url "api/v1" que la aplicación se ejecuta exitosamente.

![pw-application-is-running-yay-01.png](img/pw-application-is-running-yay-01.png)

---------------------------------

Proceder con la configuración de code pipeline, Con code pipeline se podrá desplegar la imagen de Docker generada por la etapa build.
Para ello:

1. Ir a al servicio "Code Pipeline"

![pw-code-pipe-line-service-01.png](img/pw-code-pipe-line-service-01.png)


2. Seleccionar "Crear la canalización"

![pw-pipeline-create-pipeline-01.png](img/pw-pipeline-create-pipeline-01.png)

3. Establecer parámetros del pipe de despliegue
a) Nombre del pipe, debe ser un nombre descriptivo.
b) Seleccionar "Nuevo rol de servicio"

![pw-pipeline-configure-pipe-01.png](img/pw-pipeline-configure-pipe-01.png)


3. Configurar la etapa de origen
a) Establecer como origen Code Commit
b) Establecer el nombre del repositorio, usar el que se ha usado par prácticas anteriores.
c) Establecer la rama master.
d) Establecer que Pipeline sea disparado de acuerdo a eventos de Cloud Watch. 
e) Establecer el artefacto de salida como "predeterminado", el artefacto de salida será una copia del código que Pipeline guardará en un bucket S3 para que la siguiente etapa (Build) pueda trabajar.

Click en "Sigueinte"

![pw-code-pipe-add-origin-to-pipe-01.png](img/pw-code-pipe-add-origin-to-pipe-01.png)


a) Seleccionar como proveedor de compilación a "AWS CodeBuild"
b) Seleccionar la región donde se ha trabajado todo el proyecto.
c) Seleccionar el nombre del proyecto de compilación recién creado.
d) Establecer "Compilación única"

Click en "Siguente"

![pw-code-pipe-specify-build-steps-01.png]img/(pw-code-pipe-specify-build-steps-01.png)


a) Seleccionar como proveedor de implementación "Amazon ECS"
b) Establecer la región donde se ha trabajado el proyecto.
c) Establecer el nombre del cluster ECS.
d) Establecer el servicio del cluster donde se deberá implementar el contenedor docker.
e) Establecer el valor como `imagedefinitions.json`, este es un archivo generado por AWS CodeBuild que especifica el nombre del contenedor en la tarea de ejecución y la URI de la imagen de docker a ser desplegada.
f) Establecer el tiempo de espera en unos 20 minutos.

Click en "siguiente"

![pw-code-pipe-add-deploy-step-01.png](img/pw-code-pipe-add-deploy-step-01.png)


Al pasar por el review de la aplicación y estar conforme con los parámetros se puede dar click en "Crear la canalización"

![pw-code-pipe-review-config-01.png](img/pw-code-pipe-review-config-01.png)



Un Proceso nuevo de despliegue comienza a ejecutarse:

![pw-codepipeline-deplot-application-on-pipe-creation-01.png](img/pw-codepipeline-deplot-application-on-pipe-creation-01.png)


Al iniciar el despliegue se puede observar en los eventos del Servicio en el cluster como el servicio comienza la tarea de despliegue
![pw-codepipe-ecs-deploy-service-01.png](img/pw-codepipe-ecs-deploy-service-01.png)


En los eventos del servicio se pueden ir viendo todos los pasos que orquestan el despliegue del código.

![pw-ecs-service-events-01.png](img/pw-ecs-service-events-01.png)


Después de unos 10 minutos ser ve como el proceso de despliegue es exitoso.
![pw-codepipe-is-complete-yay-01.png](img/pw-codepipe-is-complete-yay-01.png)


a) Para probar que el despliegue se ejecuta correctamente se podría modificar el archivo "api/v1/views.py" 
b) Hay una clase llamada StatusView, en el Response se puede modificar el texto que regresa.
c) Una vez modificado se puede hacer push de este código.

![pw-git-push-to-test-deploy-01.png](img/pw-git-push-to-test-deploy-01.png)


Después de algunos minutos hecho el push de código se puede comprobar como la nueva versión ha sido desplegada sin necesidad de entrar a ningún servidor.
![pw-code-deployed-tested-rest-done-01.png](img/pw-code-deployed-tested-rest-done-01.png)

