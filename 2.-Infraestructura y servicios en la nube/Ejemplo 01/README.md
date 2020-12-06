# Ejemplo 1 - Usando AWS CLI

## 1. Objetivo 
- Gestionar los recursos de AWS por línea de comandos con AWS CLI.

## 2. Requisitos 
- Sistema operativo actualizado.
- Python 3.4+ instalado. [¿Cómo instalar?](https://aws.amazon.com/es/blogs/developer/deprecation-of-python-2-6-and-python-3-3-in-botocore-boto3-and-the-aws-cli/).
- Fecha y hora correcta en el equipo local.

# 3. Desarrollo 


### Instalación en Windows:


1. En Windows basta con descargar e instalar el siguiente [MSI](https://awscli.amazonaws.com/AWSCLIV2.msi)

<img src="img/wizard-01.png"></img>

2. Aceptar términos y condiciones, después especificar el directorio de instalación.

<img src="img/wizard-02.png"></img>

3. Esperar a finalizar la instalación.

<img src="img/wizard-03.png"></img>

<img src="img/wizard-04.png"></img>


4. Verificar la instalación abriendo una consola CMD ejecutando el comando `aws --version`

<img src="img/awscli-comprobacion-windows.png"></img>

Instalación Linux:
1. Descargar el instalador para Linux con el comando:
```sh
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
```

<img src="img/wizard-01-linux.png"></img>

2. Descomprimir el zip descargado con el comando 
```sh
unzip awscliv2.zip
```
<img src="img/wizard-02-linux.png"></img>

3. Ejecutar instalador.
```bash
sudo ./aws/install
```
Ingresar el password para sudo, esperar por la instalación.

<img src="img/wizard-03-linux.png"></img>

4. Comprobar la instalación con el comando
```bash
aws --version
```
<img src="img/wizardLx.png"></img>


### Instalación en MAC:
1. Descargar el instalador pkg [desde](https://awscli.amazonaws.com/AWSCLIV2.pkg).
2. Iniciar el instalador dando doble click al archivo instalado.

-------------Configuración---------------------
Ya instalado AWS CLI, es necesario configurarlo.

1. Ejecutar el comando:
```bash
aws configure
```

<img src="img/aws-cli-configutacion.png"></img>

> **Nota:**
> 
>Se recomienda seleccionar la región con menos latencia, se puede medir con [https://ping.psa.fun](https://ping.psa.fun)

Para obtener las claves de acceso pasar a la siguiente sección del ejemplo.

2. Con el comando ```aws help``` se puede consultar la ayuda.
3. Para obtener los usuarios en la cuenta AWS el comando es:

<img src="img/aws-cli-command-01.png"></img>

4. Se puede obtener una lista de los subcomandos disponibles, por ejemplo, para obtener los subcomandos disponibles para el comando "iam" ejecutar el comando 
```bash
aws iam help
```
El punto clave es el argumento `help`

<img src="img/AWScli-iam-subcommands.png"></img>


**¿De dónde salen las claves Access Key ID y Secret Access Key?**

1. Ir a la [consola](https://console.aws.amazon.com/iam/)
2. Seleccionar Usuarios, después "Añadir usuario(s)" 

<img src="img/iam-new-user-for-cli.png"></img>

3. Especificar el nombre del usuario y seleccionar "Acceso mediante programación"

<img src="img/iam-cli-user-02.png"></img>

4. Seleccionar la política (permisos) que aplicará al usuario.
 
<img src="img/iam-aws-cli-policy.png"><img>

5. Siempre es recomendable agregar TAGS (etiquetas de identificación) cuando sea posible, ayudará para la administración.

<img src="img/awscli-iam-tags.png"><img>

6. Generado el usuario ya se pueden obtener las claves de acceso, la clave de acceso secreta solo se puede ver si se da click en "mostrar", esta es la única vez que aparecerá la clave de acceso secreta, es necesario guardarla, de preferencia en un gestor de contraseñas.

<img src="img/Awscli-claves-acceso.png"><img>
