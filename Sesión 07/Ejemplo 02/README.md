# Ejemplo 02: Elastic Container Registry & Code Build

## 1. Objetivo

- Aprender como hacer despliegue automático de un aplicación web (la misma que ya se había trabajado anteriormente) en instancias que escalan automáticamente, la aplicación se ejecutará en una instancia de Docker. La idea es que además el despliegue sea orquestado de manera automática en instancias generadas también de manera automática, dicha generación de instancias estará a cargo del servicio `Elastic Container Service`

![pw-a-donde-vamos-01.png](../img/pw-a-donde-vamos-01.png)

## 2. Requisitos 
- Acceso a AWS Console (WEB)
- Comprender como funciona un balanceador de carga
- Conocimientos básicos de Docker
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

![pw-access-to-iam--web-console-01.png](../img/pw-access-to-iam--web-console-01.png)

2. Ir a la parte de roles.

![pw-roles-main-menu-01.png](../img/pw-roles-main-menu-01.png)


3. Dar click en "Crear un rol"

![pw-iam-create-role-button-01.png](../img/pw-iam-create-role-button-01.png)

4. Seleccionar "Servicio de AWS", después seleccionar "Elastic container service", en el apartado inferior seleccionar "Elastic container Service Task", después click en "Sigueinte:Permisos"
![pw-iam-select-role-for-ec2-01.png](../img/pw-iam-select-role-for-ec2-01.png)

5. Normalmente se deberían seleccionar los permisos para el rol, por lo pronto se dejará sin seleccionar ninguna política.
![pw-iam-select-sqs-services-01.png](../img/pw-iam-select-sqs-services-01.png)

6. Añadir una etiqueta

![pw-iam-assign-tags-to-role-01.png](../img/pw-iam-assign-tags-to-role-01.png)

7. Establecer un nombre descriptivo para el rol.

![pw-sqs-assign-name-and-description-01.png](../img/pw-sqs-assign-name-and-description-01.png)

8. Finalmente se tiene el rol creado.
![pw-iam-role-created-01.png](../img/pw-iam-role-created-01.png)


## Grupos de seguridad.
Al momento se tienen grupos de seguridad para conexión a base de datos, conexión por ssh, conexión web, se requerirá un nuevo grupo de seguridad para permitir la redirección de tráfico desde el balanceador de carga a instancias EC2 por puertos dinámicos. Los puertos dinámicos son los que darán la capacidad de desplegar una imagen Docker sin comprometer una imagen ya en ejecución mientras la nueva imagen es desplegada.
Para ello:

1. Ingresar al servicio VPC
![pw-vpc-menu-access-01.png](../img/pw-vpc-menu-access-01.png)

2. Dirigirse a la sección de Grupos de seguridad
![pw-vpc-access-security-groups-01.png](../img/pw-vpc-access-security-groups-01.png)

3. Crear un nuevo grupo de seguridad
![pw-vpc-create-new-security-group-01.png](../img/pw-vpc-create-new-security-group-01.png)


4. Establecer como parámetros:
a) Un nombre para el grupo de seguridad, para este ejemplo `seg-gr-dynamic-01`
b) Establecer una descripción.
c) Establecer la VPC donde funcionará el grupo de seguridad, establecer la VPC con la que se ha venido desarrollando el proyecto.
d) Establecer un rango de puertos 49153 - 65535 con permisos de ser usados desde cualquier dirección IP (Anywhere), notar que el tipo es "Custom TPC"
e) Establecer un rango de puertos 32768 - 61000 con permisos de ser usados desde cualquier dirección IP (Anywhere), notar que el tipo es "Custom TPC"
f) Como regla de salida se puede dejar la que viene por defecto.

![pw-vpc-new-security-group-01.png](../img/pw-vpc-new-security-group-01.png)

5. Con estos pasos ya se tiene el grupo de seguridad que será usado para las instancias EC2 que serán creadas por el servicio Elastic Container Service.
![pw-vpc-security-group-done-01.png](../img/pw-vpc-security-group-done-01.png)


1. Antes de ejecutar el pipeline se deberá crear un registro para almacenar la imagen de docker que vaya resultando de la etapa de "Build", 
para ello hay que ir al servicio "Elastic Container Registry" 


![pw-ecr-search-service-01.png](../img/pw-ecr-search-service-01.png)


2. Click en **Get Started**

![pw-ecr-get-sterted-01.png](../img/pw-ecr-get-sterted-01.png)


3. a) Establecer la visibilidad del registro, se establecerá como privado, b) Seleccionar un nombre para el repositorio. 

![pw-ecr-create-repository-01.png](../img/pw-ecr-create-repository-01.png)

El repositorio es creado:

![pw-ecr-repository-created-01.png](../img/pw-ecr-repository-created-01.png)

Copiar el "Repository name", será usado en la siguiente etapa, para este ejemplo el nombre es `generador-leads`
Copiar la "URI" del repositorio generado, dicha URI será usada al configurar una `task definition` en el servicio ECS más adelante.

Para configurar la etapa de Build se deben seguir los siguientes pasos:

1. Buscar el servicio "Code Build" e ingresar a él.

![pw-cb.access-01.png](../img/pw-cb.access-01.png)



2. Click en "Crear el proyecto de compilación"

![pw-cb-create-compilation-project-01.png](../img/pw-cb-create-compilation-project-01.png)


3. Al ingresar a la configuración se deberá establecer los siguientes datos:
a) Establecer un nombre para el proyecto de compilación.
b) Establecer de donde saldrá el código a ser compilado, para ello se seleccionará el servicio "CodeCommit"
c) Se seleccionará el repositorio git.

![pw-code-build-generate-task-01.png](../img/pw-code-build-generate-task-01.png)

4. Seleccionar la rama de la que se deberá extraer el código fuente.

![pw-code-build-branches-origin-code-01.png](../img/pw-code-build-branches-origin-code-01.png)


5. Para configurar el entorno donde se construirá la imagen se deberá especificar:
a) Seleccionar imagen administrada
b) Seleccionar el sistema operativo Amazon Linux 2
c) Seleccionar "Standard"
d) Seleccionar la imagen más reciente
e) Seleccionar la imagen más reciente de la imagen
f) Seleccionar tipo de entorno "Linux"
g) Habilitar el modo "privilegiado" ya que se generará una imagen de Docker.

![pw-code-build-configure-environment-01.png](../img/pw-code-build-configure-environment-01.png)


a) Seleccionar un nuevo rol, el rol es necesario para acceder al repositorio git con el código fuente.
b) Establecer un nombre descriptivo para el rol.
c) El tiempo de espera se debe establecer en 10 minutos
d) El tiempo de espera en cola se deberá establecer en 15 minutos
e) No se debe instalar ningún certificado.

![pw-code-build-environ-config-02.png](../img/pw-code-build-environ-config-02.png)


5. a) Establecer la VPC en la cual se conectará la instancia de Code buil para generar la imagen, establecer la VPC con la que se ha trabajado el proyecto.
b) Seleccionar subredes privadas que tengan el acceso por NAT Gateway hacia internet.
c) Seleccionar un grupo de seguridad con acceso hacia internet, los grupos que se han manejado hasta ahora no tienen restricción en tráfico de salida.
d) Dar click en "Validar la configuración de la VPC", e) Se deberá mostrar un mensaje confirmando el acceso a internet.

![pw-code-build-environ-config-03.png](../img/pw-code-build-environ-config-03.png)

6. Se debe especificar el tamaño de la instancia donde se generara la imagen de docker
a) El programa no es nada pesado, se puede especificar el tamaño de la instancia en la más pequeña
b) Establecer la variable de entorno `AWS_DEFAULT_REGION` con el valor `us-east-1`
c) Establecer la variable de entorno `AWS_ACCOUNT_ID` con el valor  del número de cuenta, dicho valor puede ser consultado en el menú superior derecho.
d) Establecer la variable de entorno `IMAGE_TAG` con el valor `latest`
e) Establecer la variable de entorno `IMAGE_REPO_NAME` con el nombre del repositorio de imágenes docker recién creado.

![pw-code-build-instance-size-and-env-variables-01.png](../img/pw-code-build-instance-size-and-env-variables-01.png)

7. EL archivo de especificación es un archivo que viene en la raiz del proyecto, se llama `buildspec.yml`, en él se definen los pasos a seguir para generar la imagen docker.

![pw-code-build-compilation-settings-01.png](../img/pw-code-build-compilation-settings-01.png)

8. a) Especificar una construcción sin artefactos, b),c) Deshabilitar los logs. Finalizar dando click en "Crear el proyecto de compilación"

![pw-code-build-last-step-01.png](../img/pw-code-build-last-step-01.png)

Después de un minuto el proyecto de compilación se genera.

![pw-code-build-compilation-rpoyect-created-01.png](../img/pw-code-build-compilation-rpoyect-created-01.png)



Antes de finalizar, habrá que ir al servicio IAM a agregar un rol a la política para asegurar que la imagen resultante se puede subir al registro en el servicio ECR, para ello:

a) Buscar en los roles, el rol recién establecido en "Code Build" (en este caso role-code-build-leads-generator-01)
b) a dicho rol en la sección de "permisos"
c) Agregar la política "AmazonEC2ContainerRegistryPowerUser"
d) Una vez agregada la política se verá como ha sido agregada correctamente.

![pw-iam-add-powe-user-to-ecr-01.png](../img/pw-iam-add-powe-user-to-ecr-01.png)

Para verificar que funciona correctamente se deberá seleccionar el proyecto de compilación recién creado y dar click en "Iniciar la compilación"

![pw-code-build-start-compilation-01.png](../img/pw-code-build-start-compilation-01.png)

Se Abrirá una ventana con los datos para iniciar el proceso, basta con simplemente "Iniciar la compilación"

![pw-code-build-start-compilation-02.png](../img/pw-code-build-start-compilation-02.png)


Se iniciará el trabajo de compilación 

![pw-code-build-compilation-stage-started-01.png](../img/pw-code-build-compilation-stage-started-01.png)


Después de un par de minutos se confirma que el trabajo fue creado satisfactoriamente

![pw-code-build-job-done-01.png](../img/pw-code-build-job-done-01.png)