# Postwork

## 1. Objetivo 
- Configurar un bucket S3 con un sitio estático servido por un nombre de dominio propio.

## 2. Requisitos 
- AWS CLI instalado y funcionando. 
- Acceso a AWS Console.

## 3. Desarrollo 
1. Generar un bucket con el nombre del subdominio donde será servido el sitio estático, para el ejemplo será **app.edupractice.tk**

<img src="img/make_bucket.png">

2. Copiar el contenido del sitio estático, para este ejemplo se copiará desde un bucket S3 ya existente.

<img src="img/Copy-files-from-existing-bucket.png">

3. Habilitar el bucket para servir como servidor web sitios estáticos.

<img src="img/habilitar-bucket-s3.png">

 4. Configurar el DNS para que las peticiones sean resueltas hacia el bucket, habrá que generar un nuevo registro en la zona alojada
 
<img src="img/Zona alojada.png">

5. Click en **Crear un registro**

<img src="img/crear-registro.png">

6. Seleccionar la política de redireccionamiento como **Direccionamiento sencillo**

<img src="img/redireccionamiento-sencillo.png">

7. Dar Click en **Definir un registro**

<img src="img/configurar-registro.png">

8. Generar el subdominio del registro, en este caso "app" el nombre del registro debe coincidir con el nombre del bucket (a), especificar que el registro debe resolver un bucket de S3 (b) , notar que es una funcionalidad no estándard de DNS, es una funcionalidad añadida por tener el DNS configurado con AWS. Especificar la Zona donde trabaja el bucket  (c),  us-east-1 para el ejemplo. Seleccionar el nombre del bucket destinado a servir como servidor web estático (d), deshabilitar la evaluación de estado del destino (e).

<img src="img/configurar-registro.png">

9. Observar como el registro generado con el subdominio redirigirá el tráfico no a una dirección IP como lo haría normalmente el protocolo DNS, lo hará a un bucket S3.

<img src="img/Generar-regla.png">

10. Se verifica la generación del registro. 

<img src="img/verificar-generacion-registro.png">

11. Pasados unos minutos al hacer ping se verá que el subdominio resuelve a una dirección IP.

<img src="img/ping.png">

12. Al ingresar al dominio por le navegador se nota un error:

<img src="img/error403.png">

Esto es por que aún falta dar permiso explícitamente a los archivos en el bucket, para lo que habrá que dirigirse a los archivos en AWS Console, seleccionar todos los archivos y seleccionar **Hacer público**

<img src="img/acceso-publico.png">

13. Confirmar el acceso público.

<img src="img/aceptar-accceso-publico.png">

14. **🎉Felicidades**, el bucket esta preparado para alojar el sitio web estático del proyecto. En el futuro bastará con reemplazar los archivos.

<img src="img/is-done.png">
