# Ejemplo 2


# 1. Objetivo 🎯
- Bloquear peticiones que no se hagan desde México configurando el servicio Route53.

# 2. Requisitos 📌
- Acceso a AWS Console.
- Conocimiento básico sobre como funciona el protocolo DNS.
- Una página hosteada en S3, el nombre del bucket debe coincidir con el subdominio para hacer pruebas `geoejemlo`, así el bucket debe  llamarse para este ejemplo `geoejemlo.<sudominio>`
- Navegado Opera instalado.

# 3. Desarrollo 📑

1. Acceder al servicio Route53.

![ej2-access-route53.png](ej2-access-route53.png)


2. Seleccionar "Zona alojada"
![ej2-zona-alojada.png](ej2-zona-alojada.png)

3. Seleccionar el dominio.
![ej2-select-domain.png](ej2-select-domain.png)

4. Seleccionar "Crear un registro"
![ej2-select-create-register.png](ej2-select-create-register.png)

5. Seleccionar "Geolocalización"
![ej2-select-geo-location.png](ej2-select-geo-location.png)

6. Establecer el subdominio (a), luego seleccionar "Definir un registro de geolocalización" (b).

 ![ej2-define-georegister.png](ej2-define-georegister.png)

7. Configurar el registro de geolocalización como:
a) Seleccionar a donde será redirigido el tráfico, en este caso S3.
b) Seleccionar la región donde se encuentra el bucket deseado
c) Seleccionar el bucket.
d) Establecer la ubicación permitida.
e) Establecer un nombre representativo del registro.
![ej2-establish-configuration-geolocation-register.png](ej2-establish-configuration-geolocation-register.png)

8. Configurado el registro habrá que crearlo dando click en "Crear registros".
![ej2-create-registers.png](ej2-create-registers.png)

Segundos después se notifica la creación del registro.
![ej2-create-register-done.png](ej2-create-register-done.png)

9. Acceder al subdominio en un navegador en una sesión de incógnito.
![ej2-access-to-site-ok.png](ej2-access-to-site-ok.png)

10. Activar la VPN del navegador seleccionando "Americas", con ello se verá como al acceder desde una IP de otro país no hay acceso. 
![ej2-activate-vpn.png](ej2-activate-vpn.png)


Activada la VPN y refrescando pantalla se ve no hay acceso.
![ej2-activate-vpn-success-and-blocked.png](ej2-activate-vpn-success-and-blocked.png)

Accediendo a [https://www.iplocation.net](https://www.iplocation.net) se ve que la dirección IP está fuera de México.

![ej2-vpn-ip-non-in-mexico.png](ej2-vpn-ip-non-in-mexico.png)

