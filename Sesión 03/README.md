
# Sesión 03: Contenido y redes en AWS

## Objetivo

+ Reconocer las opciones que ofrece AWS para la gestión de redes y cómo interactúan con otros servicios, procipalmente se aprenderá a configurar un DNS para mitigar ataques de denegación de servicio y balancear cargas para tolerancia a fallas.

## Requisitos

- Acceso a AWS Console.
- Diagrama del proyecto a la mano.
- Calculadora CIDR
- Conocimiento básico sobre como funciona el protocolo DNS.
- Una página hosteada en S3, el nombre del bucket debe coincidir con el subdominio para hacer pruebas `geoejemlo`, así el bucket debe  llamarse para este ejemplo `geoejemlo.<sudominio>`
- Navegado Opera instalado.

## Organización de la clase

- [Ejemplo 01: Hostear una página web con Cloudfront](./Ejemplo%2001/README.md)
- [Ejemplo 02: Creación de VPC: Public Subnet (dmz) & Internet Gateway](./Ejemplo%2002/README.md)
- [Ejemplo 03: Creación de VPC: Subnet, NAT gateway, Elastic IP](./Ejemplo%2003/README.md)
    - [Reto 01: Creación de Security Groups.](./Reto%2001/README.md)
- [Postwork: Bloqueo y acceso de peticiones por geolocalización](./Postwork.md)
