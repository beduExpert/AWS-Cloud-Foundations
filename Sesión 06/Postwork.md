# Postwork 

## 1. Objetivo 
- Conocer el bastión básico para la ejecución de cómputo en la nube con EC2, dentro de las instancias EC2 se ejecutará un contenedor de Docker, el contenedor obtendrá los datos para guardarlos en base de datos relacional. 

![pw-a-donde-vamos-01.png](img/pw-a-donde-vamos-01.png)


## 2. Requisitos 
- Acceso a la consola de AWS (log)
- Una base de datos RDS generada, tener contraseña, usuario, url (Endpoint ) de la instancia.
- Tener grupos de seguridad de tráfico de entrada a puertos 22, 80, 443, 5432.
- Certificado de seguridad en Amazon Certificade Manager.
- [Postman](https://www.postman.com/product/rest-client/) instalado para verificar el funcionamiento de la API.

## 3. Desarrollo 

1. En el panel de EC2, dar click en "Lanzar la instancia".

![pw-launch-instance.png](img/pw-launch-instance.png)

2. Seleccionar la siguiente AMI.

![pw-select-ami.png](img/pw-select-ami.png)

3. Seleccionar el tamaño de la instancia, para este ejercicio t2.micro esta bien.

![pw-instance-size.png](img/pw-instance-size.png)


4. Configurar la instancia como:
a) Establecer el número de instancias en **2**.
b) Seleccionar la VPC con la que se ha venido trabajando.
c) Seleccionar una de las subredes públicas.
d) Establecer asignación de IP pública al momento de generarse la instancia.

![pw-configure-instance-01.png](img/pw-configure-instance-01.png)

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
![pw-configure-instance-02.png](img/pw-configure-instance-02.png)


5. Establecer el storage que tendrán las instancias, habilitar el borrado en terminación, al momento de eliminar la instancia también se eliminará el volumen.

![pw-configure-storage-01.png](img/pw-configure-storage-01.png)

6. Establecer Tags para facilidad de administración.

![pw-add-tags-01.png](img/pw-add-tags-01.png)

7. Establecer los grupos de seguridad (firewall a nivel de instancia) definiendo el tráfico permitido a las instancias. En este caso SSH, HTTP y HTTPS.

![pw-security-group-01.png](img/pw-security-group-01.png)


8. Verificar las configuraciones y lanzar las instancias.

![pw-launch-instance.png](img/pw-launch-instance.png)

9. Generar la llave de conexión nueva (a), establecer el nombre de la llave (b), descargar la llave (c) sin la llave no se podrá conectar a la instancia por SSH, finalizar en "Launch Instances" (d).

![pw-launch-instance-01.png](img/pw-launch-instance-01.png)

Después de algunos momentos las instancias son generadas.

![pw-launching-instances-01.png](img/pw-launching-instances-01.png)

-----------------------------------

Toca dar de alta el balanceador de carga, se encargará de despachar las solicitudes a cada una de las instancias lanzadas.

1. En el panel del servicio EC2, seleccionar "Balanceadores de carga" (a), después "Create Load Balancer" (b).

![pw-create-balancer-01.png](img/pw-create-balancer-01.png)

2. Seleccionar el balanceador de carga de tipo **Application**

![pw-select-application-load-balancer-01.png](img/pw-select-application-load-balancer-01.png)


3. Configurar el balanceador con:
a) Establecer un balanceador de carga.
b) Establecer un balanceador de carga de cara hacia internet
c) Seleccionar el tráfico https
d) Seleccionar la red VPC con la que se ha venido trabajando
e) Seleccionar las subredes públicas de cara a internet.

![pw-load-balancer-configure-01.png](img/pw-load-balancer-configure-01.png)

4. Seleccionar el certificado de seguridad.

![pw-add-certificate-manager-01.png](img/pw-add-certificate-manager-01.png)


5. Seleccionar el permiso para tráfico https.

![pw-security-group-load-balancer.png](img/pw-security-group-load-balancer.png)


6. Establecer el **target group**, básicamente con esto se le indica al balanceador de carga cuales son las instancias EC2 a las que debe ser redirigido el tráfico.

a), b) Especificar un nuevo target, dar un nombre específico al target para fácil administración.
c) especificar que se darán de alta en el grupo instancias.
d), e) Especificar protocolo y puerto al cual será redirigido el tráfico hacia las instancias.
f) Para este caso especial por el tipo de programa que se usará, especificar el protocolo http y la ruta `/api/v1/` como path para verificar que el servicio se este ejecutando.

![pw-create-target-group.png](img/pw-create-target-group.png)
 
7. Agregar las instancias que se agregaran al target group para redirigir el tráfico web.
a) Seleccionar las instancias.
b) Agregar las instancias al balanceador.

![pw-add-instances-to-target-group.png](img/pw-add-instances-to-target-group.png)

Pocos segundos después el balanceador es generado.

 ![pw-balancer-created-01.png](img/pw-balancer-created-01.png)


--------------------------------------------------------------
Generado el balanceador de carga ahora hay que desplegar el código que ejecutará cada una de las instancias.
1. Para ingresar a la instancia hay que ir al panel de EC2, luego a la sección "Instancias", dar click en una instancia (b) y dar click en "Conectar" (b)

![pw-connect-instance-01.png](img/pw-connect-instance-01.png)

2. Seleccionar "Conexión de la instancia EC2" (a), luego "Conectar" (b).

![pw-instance-connection-02.png](img/pw-instance-connection-02.png)


La conexión se establece en unos segundos a una consola bash conectada a la instancia.

![pw-bash-console.png](img/pw-bash-console.png)

3. Habrá que corroborar el acceso a la base de datos generada con anterioridad.
Para lo cual habrá que ejecutar el comando 
```ssh
psql -h <endpoint del host> -U <usuario_de_base_datos>
```

Ejemplo: ![pw-check-sql-connexion.png](img/pw-check-sql-connexion.png)

**¡En este caso hay un problema!**, no se logra la conexión con la base de datos, al no lograr conexión, lo primero que hay que hay que verificar son los grupos de seguridad, recordar que los grupos de seguridad actuan como un firewall, para lo que habrá que dirigirse a la sección de RDS.

![pw-redirect-to-rds-panel.png](img/pw-redirect-to-rds-panel.png)

- Click en **Databases**

![pw-databases-ckick-on.png](img/pw-databases-ckick-on.png)

- Seleccionar al instancia y luego dar click en **Modify**

![pw-modify-instance.png](img/pw-modify-instance.png)

- Hacer scroll hasta la parte de **Connectivity**, aquí se observa el problema, se tiene un grupo de seguridad por defecto, habrá que cambiar el grupo para explícitamente permitir el tráfico al puerto 5432.

![pw-bad-security-group.png](img/pw-bad-security-group.png)

- Se selecciona el grupo de seguridad adecuado para el tráfico de Postgres en el puerto 5432.
![pw-modify-connectivity.png](img/pw-modify-connectivity.png)

- Hacer scroll al final de la pantalla y dar click en **Continuar**
![pw-rds-continue-modify.png](img/pw-rds-continue-modify.png)

- Especificar que los cambios se generen inmediatamente, luego dar click en **Modify DB instance**.

![pw-modify-rds-instance-start.png](img/pw-modify-rds-instance-start.png)

- El cambio es prácticamente instantáneo.
 ![pw-rds-modify-rds-instance-done.png](img/pw-rds-modify-rds-instance-done.png)


Regresando a la linea de comandos (si deja de responder solo refrescar pantalla con F5) se puede corroborar la conexión a la instancia de base de datos, incluso se pueden listar las bases de datos, tener el nombre de la base de datos en cuenta.

![pw-database-connexion-done.png](img/pw-database-connexion-done.png)

4. Para seguir con el ejemplo, se deberá ejecutar en la linea de comando el siguiente comando
```ssh
docker run -p 80:8000 -e DB_HOST=db-app-01-prod-01.cj0gk32kblft.us-east-1.rds.amazonaws.com -e DB_NAME=clientes_prod_db_01 -e DB_USER=postgres -e DB_PASSWORD=<el-password-de-la-base> -it bay007/edu
```

docker run: Indica que se deberá ejecutar un comando de docker
En las variables de entorno DB_HOST, DB_NAME, DB_USER, DB_PASSWORD se deberán establecer los parámetros propios para la conexión con la base de datos.

![pw-run-docker-instance-01.png](img/pw-run-docker-instance-01.png)

Ejecutado el comando se tendrá un contenedor de Docker con el código fuente funcionando.

![pw-docker-running-instance-01.png](img/pw-docker-running-instance-01.png)

5. La ejecución del contenedor de Docker se debe efectuar en la otra instancia de EC2, para lo que hay que conectarse a ella y ejecutar el mismo comando de Docker del paso 4.

![pw-docker-running-instance-02.png](img/pw-docker-running-instance-02.png)

Pasados algunos minutos se comienza a observar tráfico hacia la instancia EC2 y en consecuencia a los contenedores ejecutándose. Este tráfico es generado por el balanceador de carga probando la salud del servicio.

![pw-health-check-load-balancer.png](img/pw-health-check-load-balancer.png)


6. Ahora como paso siguiente se se debe configurar el balanceador de carga con un subdominio, ya que las peticiones web no llegarán directamente a las instancias de EC2 y a los contenedores que estan dentro de ellas, quien recibe el tráfico HTTP y HTTPS será el balanceador de carga, y para poder llegar a él se debe configurar un subdominio, para lo cual se debe ir al servicio Route 53.

![pw-ingress-route-53.png](img/pw-ingress-route-53.png)

7. Click en **Zonas alojadas**

![pw-zonas-alojadas-01.png](img/pw-zonas-alojadas-01.png)

8. Entrar al  dominio para su modificación.

![pw-select-domain-for-configure.png](img/pw-select-domain-for-configure.png)

9. Click en **Crear registro**

![pw-create-register-01.png](img/pw-create-register-01.png)

10. Seleccionar **Direccionamiento sencillo**

![pw-route53-create-registry-01.png](img/pw-route53-create-registry-01.png)

11. Click en **Definir un registro simple**
![pw-define-simple-register.png](img/pw-define-simple-register.png)

12. Configurar el registro como:
a) Establecer el subdominio como `api`
b) Buscar el servicio de "Balanceo de carga clásico y de aplicaciones"
c) Seleccionar la región donde se encuentra el balanceador de carga.
d) Seleccionar el balanceador de carga, hay que notar el dominio del balanceador, por ello importante nombrar los recursos.
e) Definir redirigir el tráfico a IPv4
f) Deshabilitar la opción.

![pw-route-53-configure-registro-01.png](img/pw-route-53-configure-registro-01.png)

13. Click en **Crear registros**

![pw-create-route53-register-02.png](img/pw-create-route53-register-02.png)

Segundos después el registro es creado
![pw-route53-regiser-done.png](img/pw-route53-regiser-done.png)

14. Cinco minutos después de generado el registro al hacer ping ya hay una resolución del subdominio `api`, esperar hasta que el ping tenga resolución a una IP. (Notar que el balanceador no responderá al ping, es normal ya que el balanceador solo tiene permitido por el grupo de seguridad y la propia configuracion del balanceador majenar tráfico de los puertos 80 y 443 es decir HTTP y HTTPS)

![pw-ping-done.png](img/pw-ping-done.png)

15. Con Postman se comenzará la prueba de funcionamiento de la API. 
a) Seleccionar el protocolo POST.
b) Establecer la la URL a la que se hará el request, conformada por `https://` más el subdominio recién configurado en Route 53 más el path `/api/v1/leads/` (poner atención en la diagonal al final).
c) Establecer la petición como `raw`.
d) Establecer el tipo de petición como `JSON`.
e) Establecer el cuerpo del mensaje como:
```json
{
    "name": "Carlos Perez",
    "email": "carlos.perez@yopmail.com",
    "subject": "Cotización",
    "message":"Hola, buenas tardes, solo quiero saber el costo de servicio por declaración de impuestos anual"
}
```
f) Generar la petición.

![pw-make-petition-request.png](img/pw-make-petition-request.png)

Hecha la petición se ve el código de estatus 201 lo que indica que el registro fue creado en la base de datos, además del ID que la base ha asignado al registro.

![pw-postman-request-done.png](img/pw-postman-request-done.png)

Viendo la consola se puede ver la petición recién hecha.

![pw-request-done-on-cli.png](img/pw-request-done-on-cli.png)


---------------------------------------------

Ya corroborado el funcionamiento de la API, se procede con la descarga del código fuente HTML que estará de cara al usuario final.

1. Con el código fuente descargado, descomprimir el zip, abrir el archivo `index.html` en un editor como Notepad, Vim, VSCode, SublimeText, Atom, Notepad++ o cualquier otro.
2. Ir a la siguiente sección de código para establecer la URL de la API, misma que fue usada en Postman.

![pw-edit-domain-01.png](img/pw-edit-domain-01.png)

En el ejemplo trabajado se tiene la URL, el alumno deberá establecer la URL con su propio dominio.

![pw-establish-domian-html.png](img/pw-establish-domian-html.png)

Se guarda el archivo.

3. Subir los archivos al bucket S3.

![pw-s3-select-on-console.png](img/pw-s3-select-on-console.png)

4. Seleccionar el bucket que hostea la página web.

![pw-hosting-update-files.png](img/pw-hosting-update-files.png)

5. Dar click en "Cargar" (a), arrastrar los archivos y carpetas, dar click en "Cargar" (b).

![pw-load-files-html.png](img/pw-load-files-html.png)

6. Ya subidos los archivos, acceder al subdominio `app`, se debe visualizar un formulario de captación de datos que al modificar la URL ya guardará los datos en la base de datos. 

![pw-new-form-add-customers.png](img/pw-new-form-add-customers.png)

Al hacer click en **Suscribirse** los datos serán guardados.

![pw-suscription-done.png](img/pw-suscription-done.png)


**🎉¡Felicidades!**, ha sido un trabajo arduo llegar hasta este punto, el estado del proyecto es el siguiente:

![pw-Proyecto-status.jpg](img/pw-Proyecto-status.jpg)
