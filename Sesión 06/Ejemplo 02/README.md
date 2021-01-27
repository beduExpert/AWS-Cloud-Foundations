# Ejemplo 2 

## 1. Objetivo 
- Conocer el bastión básico para la ejecución de cómputo en la nube con EC2. 

![pw-a-donde-vamos-01.png](../img/pw-a-donde-vamos-01.png)


## 2. Requisitos 
- Acceso a la consola de AWS (log)
- Una base de datos RDS generada, tener contraseña, usuario, url (Endpoint ) de la instancia.
- Tener grupos de seguridad de tráfico de entrada a puertos 22, 80, 443, 5432.
- Certificado de seguridad en Amazon Certificade Manager.

## 3. Desarrollo 

1. En el panel de EC2, dar click en "Lanzar la instancia".

![pw-launch-instance.png](../img/pw-launch-instance.png)

2. Seleccionar la siguiente AMI.

![pw-select-ami.png](../img/pw-select-ami.png)

3. Seleccionar el tamaño de la instancia, para este ejercicio t2.micro esta bien.

![pw-instance-size.png](../img/pw-instance-size.png)


4. Configurar la instancia como:
a) Establecer el número de instancias en **2**.
b) Seleccionar la VPC con la que se ha venido trabajando.
c) Seleccionar una de las subredes públicas.
d) Establecer asignación de IP pública al momento de generarse la instancia.

![pw-configure-instance-01.png](../img/pw-configure-instance-01.png)

e) Establecer el comportamiento del apagado de la instancia como "Stop"
f) Habilitar la protección contra borrado accidental (se recomienda siempre habilitarla).
g) Establecer la ejecución en hardware compartido.
h) Establecer comandos que se deben ejecutar al momento de crear la instancia, copiar y pegar los siguientes comandos (debe ir desde el # hasta la última palabra).
```ssh
#!/bin/bash
sudo yum update -y
sudo amazon-linux-extras install -y docker
sudo yum install -y postgresql
sudo service docker start
sudo usermod -a -G docker ec2-user
sudo systemctl enable docker
```
![pw-configure-instance-02.png](../img/pw-configure-instance-02.png)


5. Establecer el storage que tendrán las instancias, habilitar el borrado en terminación, al momento de eliminar la instancia también se eliminará el volumen.

![pw-configure-storage-01.png](../img/pw-configure-storage-01.png)

6. Establecer Tags para facilidad de administración.

![pw-add-tags-01.png](../img/pw-add-tags-01.png)

7. Establecer los grupos de seguridad (firewall a nivel de instancia) definiendo el tráfico permitido a las instancias. En este caso SSH, HTTP y HTTPS.

![pw-security-group-01.png](../img/pw-security-group-01.png)


8. Verificar las configuraciones y lanzar las instancias.

![pw-launch-instance.png](../img/pw-launch-instance.png)

9. Generar la llave de conexión nueva (a), establecer el nombre de la llave (b), descargar la llave (c) sin la llave no se podrá conectar a la instancia por SSH, finalizar en "Launch Instances" (d).

![pw-launch-instance-01.png](../img/pw-launch-instance-01.png)

Después de algunos momentos las instancias son generadas.

![pw-launching-instances-01.png](../img/pw-launching-instances-01.png)