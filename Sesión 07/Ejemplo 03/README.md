# Ejemplo 03: Elastic Container Service & Task Definition

## 1. Objetivo

- Definir dónde se ejecutará la imagen de docker generada por AWS CodeBuild

## 2. Requisitos 
- Acceso a AWS Console
- Haber completado el Ejemplo 02

## 3. Desarrollo

1. Se buscará el servicio ECS (Elastic Container Service)  y se ingresará en él.

![pw-ecs-acces-from-admin-web-01.png](../img/pw-ecs-acces-from-admin-web-01.png)

2. Dar click en la sección "Clusters"

![pw-ecs-container-service-principal-menu-01.png](../img/pw-ecs-container-service-principal-menu-01.png)

3. Dar click en "Create Cluster"

![pw-ecs-create-cluster-specific-menu-01.png](../img/pw-ecs-create-cluster-specific-menu-01.png)

4. Seleccionar EC2 Linux + Networking

![pw-ecs-select-ec2-and-network-01.png](../img/pw-ecs-select-ec2-and-network-01.png)

5. Configurar cluster
```
a) Establecer un nombre al cluster
b) Establecer el tamaño de instancia donde se ejecutará la imagen de docker, en este caso t2.medium
c) Establecer el número de instancias en 2
d) Establecer la imagen del sistema operativo del Host, en este caso Amazon Linux 2 AMI
e) Establecer el volumen de disco duro, el mínimo son 30 GB, es suficiente para el ejercicio.
f) No se requerirá acceder a las instancias que ECS genere, no seleccionar ninguna llave.
```

![pw-ecs-create-cluster-set-ec2-size-01.png](../img/pw-ecs-create-cluster-set-ec2-size-01.png)

6. Seleccionar la red VPC donde se establecerán las instancias (seleccionar la VPC que se ha trabajado en todo el proyecto), establecer las subredes públicas donde residirán las instancias creadas, se establece que se debe asignar una IP pública y se debe seleccionar el grupo de seguridad recién configurado diseñado para puertos dinámicos.

![pw-ecs-set-networking-01.png](../img/pw-ecs-set-networking-01.png)

7. Establecer en el punto a) un nuevo rol que ECS creará automáticamente, b) asignar una etiqueta solo para identificación.

![pw-ecs-set-role-and-tags-01.png](../img/pw-ecs-set-role-and-tags-01.png)

En ese momento el cluster comienza a generarse.

![pw-ecs-cluster-creating-01.png](../img/pw-ecs-cluster-creating-01.png)

Al dar click en "View Cluster" se podrá ver el custer generado... después de aproximadamente 5 minutos se verán nuevas instancias EC2 generadas automáticamente, en estas instancias es donde se ejecutará la imagen docker creada por AWS Code Build.

![pw-ecs-cluster-instances-created-done-01.png](../img/pw-ecs-cluster-instances-created-done-01.png)

Una vez generado el cluster, se debe configurar una "tarea",  en una tarea se definen las reglas bajo las cuales el o los contenedores operarán, se puede ver como un template o plantilla, más tarde se asociará esta plantilla con el cluster de instancias EC2 recién generado por medio de un _servicio_.

1. a) Ir a "Task definitions", b) después dar click en "Create new Task Definition"

![pw-ecs-tas-definition-01.png](../img/pw-ecs-tas-definition-01.png)

2. Seleccionar una tarea compatible con instancias EC2.

![pw-ecs-select-ec2-type-task-01.png](../img/pw-ecs-select-ec2-type-task-01.png)

3. Configurar la tarea
```
a) Establecer un nombre de la tarea descriptivo.
b) Establecer el rol que permitirá a los contenedores comunicarse con otros servicios de AWS. Se generó con anterioridad.
c) El modo de red debe quedar como "Bridge"
```

![pw-ecs-configure-task-01.png](../img/pw-ecs-configure-task-01.png)

```
a) Establecer el Rol de ejecución como "create new role"
b) Establecer el tamaño de la memoria tarea de ejecución en 256 MiB
c) Establecer el tamaño de CPU de la tarea de ejecución en 1024 unidades
```

![pw-ecs-configure-task-02.png](../img/pw-ecs-configure-task-02.png)

a) Se procederá a configurar el contenedor que será ejecutado en esta tarea

![pw-ecs-configure-task-03.png](../img/pw-ecs-configure-task-03.png)

```
a) Establecer el nombre del contenedor como `generados-leads-01`, es importante establecerlo así ya que el código fuente hace referencia a este nombre de contenedor en el archivo buildspec.yml
b) Establecer la URI de la imagen que se generó al configurar el repositorio en ECR, a la URI habrá que agregarle al final la etiqueta de la imagen de docker `:latest`
c) Establecer un limite de memoria hard de 256 MiB
d) Establecer un mapeo dinámico de puertos poniendo como _Host port_ el puerto 0, el puerto _Container port_ deberá ser establecido en 8000, 8000 es el puerto donde la aplicación se ejecuta.
```

![pw-ecs-task-config-container-01.png](../img/pw-ecs-task-config-container-01.png)

El contenedor de docker como se se ha comprobado en ejercicios anteriores requiere algunas variables de entorno para poder ser ejecutado.
```
a) Se debe establecer la variable `DB_HOST`  con el valor de la instancia RDS de base de datos.
b)  Se debe establecer la variable `DB_NAME`  con el valor especificado al generar la instancia de base de datos RDS.
c)  Se debe establecer la variable `DB_USER`  con el valor  especificado al generar la instancia de base de datos RDS.
d)  Se debe establecer la variable `REGION`  con el valor de la región donde se esté trabajando y configurando toda la aplicación.
e)  Se debe establecer la variable `DB_PASSWORD`  con el valor  especificado al generar la instancia de base de datos RDS.
```

![pw-ecs-task-config-container-02.png](../img/pw-ecs-task-config-container-02.png)

Los otros parámetros del contenedor se pueden dejar por defecto. Click en "Add"

![pw-ecs-task-config-container-05.png](../img/pw-ecs-task-config-container-05.png)

Los parámetros de la tarea de ejecución que siguen para configurar deben quedar como se aprecia en la imagen.
 Click en "Create"

![pw-ecs-task-configure-task-done-01.png](../img/pw-ecs-task-configure-task-done-01.png)
