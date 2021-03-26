
# Sesión 04: Seguridad y compliance

## Objetivo

+ Identificar los principales servicios de AWS relacionados con tópicos de seguridad, entre ellos IAM para permisos a recursos, cifrado de datos y detección de información sensible.

## Requisitos

- AWS CLI instalado configurado y funcionando.
- Un bucket previamente configurado como servidor web estático funcional.
- Un bucket S3 con algunos archivos que simularán ser archivos con información sensible.
- Una cuenta de usuario de IAM con una **política insertada**, es decir una política agregada directamente al usuario.

## Organización de la clase

- [Ejemplo 01: Acceso a S3 con usuario IAM](./Ejemplo%2001/README.md)
- [Ejemplo 02: Cifrado de datos en un S3](./Ejemplo%2002/README.md)
    - [Reto 01: Búsqueda de información sencible con Amazon Macie](./Reto%2001/README.md)
- [Ejemplo 03: Certificado SSL](./Ejemplo%2003/README.md)
- [Postwork: Creación de política insertada en IAM](./Postwork.md)
