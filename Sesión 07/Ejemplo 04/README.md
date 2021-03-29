# Ejemplo 04: ECS Cluster

## 1. Objetivo

- Generar servicio en el cluster

## 2. Requisitos 
- Acceso a AWS Console
- Haber completado el Ejemplo 03

## 3. Desarrollo 

El servicio se encargará de tomar la tarea recién definida y ejecutarla en las instancias EC2 que ha desplegado el cluster.

1. Ir al cluster que se ha generado con anterioridad

![pw-ecs-configure-service-01.png](../img/pw-ecs-configure-service-01.png)

2. a) Seleccionar "Services", b) Click en el botón "Create"

![pw-ecs-configure-service-02.png](../img/pw-ecs-configure-service-02.png)

3. Para configurar el servicio se debe establecer los siguientes valores.
```
a) El servicio será de tipo "EC2"
b) Seleccionar la `task definition` recién creada, c) con la revisión que tenga la palabra (latest)
d) Seleccionar el nombre del cluster, seleccionar el cluster recien creado
e) Establecer un nombre descriptivo para el servicio, en este caso será `servicio-generador-leads-01`
f) En tipo de servicio establecer "REPLICA"
g) En el número de tareas a ejecutar establecer 2.
h) Establecer el mínimo de instancias saludables al 50%
i) Establecer el máximo de instancias saludables al 100%
```
![pw-ecs-configure-service-03.png](../img/pw-ecs-configure-service-03.png)

```
a) En el tipo de despliegue establecer "Rolling update"
b) En Task Placement establecer "One Task Per Host"
Dar clicn en "Next step"
```
![pw-ecs-configure-service-04.png](../img/pw-ecs-configure-service-04.png)

```
a) Establecer el tipo de balanceador de carga como Application Load Balancer (anteriormente se había generado uno en el módulo "Cómputo en la nube")
b) Seleccionar "Create new role"
c) Seleccionar el balanceador de carga que se encargará de redirigir el tráfico.
d) Click en "Add to load balancer"
```
![pw-ecs-config-network-02.png](../img/pw-ecs-config-network-02.png)

Como paso siguiente seleccionar el "Target Group" asociado al balanceador de carga, luego click en "Next step"

![pw-ecs-task-config-network-04.png](../img/pw-ecs-task-config-network-04.png)

Se puede dejar en blanco las políticas de Auto Scaling, click en "Next Step"

![pw-ecs-config-network-06.png](../img/pw-ecs-config-network-06.png)

Al ver el resumen de todo lo que se configurará y estar confirme se puede dar click en "Create Service"

![pw-ecs-configure-task-done-01.png](../img/pw-ecs-configure-task-done-01.png)


Hay que esperar a que el servicio sea generado tarda pocos segundos, se puede dar click en "View Service" al finalizar. 

![pw-ecs-task-building-01.png](../img/pw-ecs-task-building-01.png)

Al dirigirse al servicio "EC2" a la sección de "Target Groups", se podrá apreciar como el servicio ejecutándose ECS ha asociado correctamente las instancias del cluster con el balanceador de carga.

![pw-ec2-target-group-instances-asociated-01.png](../img/pw-ec2-target-group-instances-asociated-01.png)

Pasados algunos minutos se podrá comprobar al ingresar a la URL del balanceador de carga en el path de la url "api/v1" que la aplicación se ejecuta exitosamente.

![pw-application-is-running-yay-01.png](../img/pw-application-is-running-yay-01.png)