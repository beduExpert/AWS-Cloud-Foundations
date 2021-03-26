# Sesión 06: Cómputo en la nube con AWS

## Objetivo

- Explicar el contexto histórico y la linea de evolución de la tecnología de servidores hasta este punto.

## Requisitos

- AWS CLI instalado y configurado.
- Tener instalado [AWS SAM CLI](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/serverless-sam-cli-install.html), ayudará a hacer el despliegue sobre AWS Lambda y API Gateway más fácil al ser un framework, SAM significa Serverless Application Model.
- [Git instalado](https://git-scm.com).
- Conocimiento básico de la consola de comandos del sistema operativo que se use.
- [NodeJS instalado](https://nodejs.org/en/download/).
- Una función lambda desplegada.
- Acceso a la consola de AWS (log)
- Una base de datos RDS generada, tener contraseña, usuario, url (Endpoint ) de la instancia.
- Tener grupos de seguridad de tráfico de entrada a puertos 22, 80, 443, 5432.
- Certificado de seguridad en Amazon Certificade Manager.
- [Postman](https://www.postman.com/product/rest-client/) instalado para verificar el funcionamiento de la API.

## Organización de la clase

- [Ejemplo 01: AWS SAM CLI](./Ejemplo%2001/README.md)
    - [Reto 01: Modificar Lambda desplegada](./Reto%2001/README.md)
- [Ejemplo 02: EC2 (bastión)](./Ejemplo%2002/README.md)
- [Ejemplo 03: Load Balancer](./Ejemplo%2003/README.md)
- [Ejemplo 04: Docker en EC2 (API del proyecto)](./Ejemplo%2004/README.md)
    - [Reto 02: Frontend para registro](./Reto%2002/README.md)
- [Postwork: Elastic Beanstalk](./Postwork.md)
