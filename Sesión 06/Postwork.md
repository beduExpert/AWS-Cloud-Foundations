# Postwork: AWS SAM CLI

## 1. Objetivo 
- Explorar el modelo de computación en la nube Serverless.

## 2. Requisitos
- AWS CLI instalado y configurado.
- Tener instalado [AWS SAM CLI](https://docs.aws.amazon.com/serverless-application-model/latest/developerguide/serverless-sam-cli-install.html), ayudará a hacer el despliegue sobre AWS Lambda y API Gateway más fácil al ser un framework, SAM significa Serverless Application Model.
- [Git instalado](https://git-scm.com).
- Conocimiento básico de la consola de comandos del sistema operativo que se use.
- [NodeJS instalado](https://nodejs.org/en/download/).


## 3. Desarrollo 

1. Verificar si se tiene instalado SAM CLI. Ejecutar el comando `sam`

<img src="img/ej1-sam-check-if-installed.png"></img>

2. Generar una carpeta especial como espacio de trabajo, al tener la carpeta navegar hasta ella y ejecutar `sam init`

<img src="img/ej1-sam-init.png"></img>

3. Seleccionar un template Quick Start (a), seleccionar un lenguaje de prrogramación para este ejemplo será la opción 1 nodejs (b), asignar un nombre del proyecto (c).

<img src="img/ej1-configure-sam-01.png"></img>

4. Un template básico será clonado desde un repositorio remoto (por eso se requiere git instalado) (a), después seleccionar el template "Hello World" (b).

<img src="img/ej1-descargando-aplicacion.png"></img>

5. Hay que leer el README.md sugerido en la pantalla anterior, dentro del readme se encuentran los comandos para despliegue de la aplicación.

<img src="img/ej1-read-readme.md.png"></img>

6. Ejecutar los comandos `sam build` en la carpeta del proyecto generado.

<img src="img/ej1-sam-build.png"></img>

7.  Ejecutar el comando `sam deploy --guided`
Se ejecutará un pequeño Wizard que habrá que ir siguiendo (a).
b) Establecer un nombre descriptivo.
c) Confirmar cambios para despliegue.

<img src="img/ej1-deploy-serverless-01.png"></img>

Por el momento dar click en yes a todas las opciones para poder desplegar la aplicación de ejemplo.
Establecer el nombre de un "ambiente", para este ejemplo se establece como `develop`. El despliegue comienza, se muestra un resumen de los cambios.

<img src="img/ej1-deploying-application-01.png"></img>

8. Desplegar los cambios presionando "Y"

<img src="img/ej1-sam-deploying-03.png"></img>

9. Al finalizar el despliegue se muestra una URL, esta URL es la que se podrá usar para consumir el servicio.

<img src="img/ej1-configuration-done-01.png"></img>

10. Al consumir desde el navegador la URL anterior se ve una respuesta de la aplicación.

<img src="img/ej1-json-responsed-sam-serverless.png"></img>

11. Ir a la consola de AWS al servicio Lambda para ver que sucedió.

<img src="img/ej1-goto-aws-console-lambda.png"></img>

12. Al abrir la consola de Lambda, se verá una lambda desplegada.

<img src="img/ej1-lambda-deployed-in-console-01.png"></img>

------------------------------

## Modificar código desplegado

Ingresar al detalle de la lambda desplegada, modificar el código fuente y hacer el nuevo despliegue de esta versión de código.

<img src="img/r1-lambda-detail.png">
