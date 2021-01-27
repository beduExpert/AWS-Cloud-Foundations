# Ejemplo 4 

## 1. Objetivo 
- Configurar las redes para conexión a base de datos

![pw-a-donde-vamos-01.png](../img/pw-a-donde-vamos-01.png)


## 2. Requisitos 
- Acceso a la consola de AWS (log)
- Una base de datos RDS generada, tener contraseña, usuario, url (Endpoint ) de la instancia.
- Tener grupos de seguridad de tráfico de entrada a puertos 22, 80, 443, 5432.
- Certificado de seguridad en Amazon Certificade Manager.
- [Postman](https://www.postman.com/product/rest-client/) instalado para verificar el funcionamiento de la API.

Generado el balanceador de carga ahora hay que desplegar el código que ejecutará cada una de las instancias.
1. Para ingresar a la instancia hay que ir al panel de EC2, luego a la sección "Instancias", dar click en una instancia (b) y dar click en "Conectar" (b)

![pw-connect-instance-01.png](../img/pw-connect-instance-01.png)

2. Seleccionar "Conexión de la instancia EC2" (a), luego "Conectar" (b).

![pw-instance-connection-02.png](../img/pw-instance-connection-02.png)


La conexión se establece en unos segundos a una consola bash conectada a la instancia.

![pw-bash-console.png](../img/pw-bash-console.png)

3. Habrá que corroborar el acceso a la base de datos generada con anterioridad.
Para lo cual habrá que ejecutar el comando 
```ssh
psql -h <endpoint del host> -U <usuario_de_base_datos>
```

Ejemplo: ![pw-check-sql-connexion.png](../img/pw-check-sql-connexion.png)

**¡En este caso hay un problema!**, no se logra la conexión con la base de datos, al no lograr conexión, lo primero que hay que hay que verificar son los grupos de seguridad, recordar que los grupos de seguridad actuan como un firewall, para lo que habrá que dirigirse a la sección de RDS.

![pw-redirect-to-rds-panel.png](../img/pw-redirect-to-rds-panel.png)

- Click en **Databases**

![pw-databases-ckick-on.png](../img/pw-databases-ckick-on.png)

- Seleccionar al instancia y luego dar click en **Modify**

![pw-modify-instance.png](../img/pw-modify-instance.png)

- Hacer scroll hasta la parte de **Connectivity**, aquí se observa el problema, se tiene un grupo de seguridad por defecto, habrá que cambiar el grupo para explícitamente permitir el tráfico al puerto 5432.

![pw-bad-security-group.png](../img/pw-bad-security-group.png)

- Se selecciona el grupo de seguridad adecuado para el tráfico de Postgres en el puerto 5432.
![pw-modify-connectivity.png](../img/pw-modify-connectivity.png)

- Hacer scroll al final de la pantalla y dar click en **Continuar**
![pw-rds-continue-modify.png](../img/pw-rds-continue-modify.png)

- Especificar que los cambios se generen inmediatamente, luego dar click en **Modify DB instance**.

![pw-modify-rds-instance-start.png](../img/pw-modify-rds-instance-start.png)

- El cambio es prácticamente instantáneo.
 ![pw-rds-modify-rds-instance-done.png](../img/pw-rds-modify-rds-instance-done.png)


Regresando a la linea de comandos (si deja de responder solo refrescar pantalla con F5) se puede corroborar la conexión a la instancia de base de datos, incluso se pueden listar las bases de datos, tener el nombre de la base de datos en cuenta.

![pw-database-connexion-done.png](../img/pw-database-connexion-done.png)

4. Para seguir con el ejemplo, se deberá ejecutar en la linea de comando el siguiente comando
```ssh
docker run -p 80:8000 -e DB_HOST=db-app-01-prod-01.cj0gk32kblft.us-east-1.rds.amazonaws.com -e DB_NAME=clientes_prod_db_01 -e DB_USER=postgres -e DB_PASSWORD=<el-password-de-la-base> -it bay007/edu
```

docker run: Indica que se deberá ejecutar un comando de docker
En las variables de entorno DB_HOST, DB_NAME, DB_USER, DB_PASSWORD se deberán establecer los parámetros propios para la conexión con la base de datos.

![pw-run-docker-instance-01.png](../img/pw-run-docker-instance-01.png)

Ejecutado el comando se tendrá un contenedor de Docker con el código fuente funcionando.

![pw-docker-running-instance-01.png](../img/pw-docker-running-instance-01.png)

5. La ejecución del contenedor de Docker se debe efectuar en la otra instancia de EC2, para lo que hay que conectarse a ella y ejecutar el mismo comando de Docker del paso 4.

![pw-docker-running-instance-02.png](../img/pw-docker-running-instance-02.png)

Pasados algunos minutos se comienza a observar tráfico hacia la instancia EC2 y en consecuencia a los contenedores ejecutándose. Este tráfico es generado por el balanceador de carga probando la salud del servicio.

![pw-health-check-load-balancer.png](../img/pw-health-check-load-balancer.png)


6. Ahora como paso siguiente se se debe configurar el balanceador de carga con un subdominio, ya que las peticiones web no llegarán directamente a las instancias de EC2 y a los contenedores que estan dentro de ellas, quien recibe el tráfico HTTP y HTTPS será el balanceador de carga, y para poder llegar a él se debe configurar un subdominio, para lo cual se debe ir al servicio Route 53.

![pw-ingress-route-53.png](../img/pw-ingress-route-53.png)

7. Click en **Zonas alojadas**

![pw-zonas-alojadas-01.png](../img/pw-zonas-alojadas-01.png)

8. Entrar al  dominio para su modificación.

![pw-select-domain-for-configure.png](../img/pw-select-domain-for-configure.png)

9. Click en **Crear registro**

![pw-create-register-01.png](../img/pw-create-register-01.png)

10. Seleccionar **Direccionamiento sencillo**

![pw-route53-create-registry-01.png](../img/pw-route53-create-registry-01.png)

11. Click en **Definir un registro simple**
![pw-define-simple-register.png](../img/pw-define-simple-register.png)

12. Configurar el registro como:
a) Establecer el subdominio como `api`
b) Buscar el servicio de "Balanceo de carga clásico y de aplicaciones"
c) Seleccionar la región donde se encuentra el balanceador de carga.
d) Seleccionar el balanceador de carga, hay que notar el dominio del balanceador, por ello importante nombrar los recursos.
e) Definir redirigir el tráfico a IPv4
f) Deshabilitar la opción.

![pw-route-53-configure-registro-01.png](../img/pw-route-53-configure-registro-01.png)

13. Click en **Crear registros**

![pw-create-route53-register-02.png](../img/pw-create-route53-register-02.png)

Segundos después el registro es creado
![pw-route53-regiser-done.png](../img/pw-route53-regiser-done.png)

14. Cinco minutos después de generado el registro al hacer ping ya hay una resolución del subdominio `api`, esperar hasta que el ping tenga resolución a una IP. (Notar que el balanceador no responderá al ping, es normal ya que el balanceador solo tiene permitido por el grupo de seguridad y la propia configuracion del balanceador majenar tráfico de los puertos 80 y 443 es decir HTTP y HTTPS)

![pw-ping-done.png](../img/pw-ping-done.png)

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

![pw-make-petition-request.png](../img/pw-make-petition-request.png)

Hecha la petición se ve el código de estatus 201 lo que indica que el registro fue creado en la base de datos, además del ID que la base ha asignado al registro.

![pw-postman-request-done.png](../img/pw-postman-request-done.png)

Viendo la consola se puede ver la petición recién hecha.

![pw-request-done-on-cli.png](../img/pw-request-done-on-cli.png)
