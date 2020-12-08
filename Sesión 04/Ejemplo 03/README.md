# Ejemplo 3 

## 1. Objetivo 
- En un bucket S3 buscar información sensible.

## 2. Requisitos 
- AWS CLI instalado y configurado.
- Un bucket S3 con algunos archivos que simularán ser archivos con información sensible.

## 3. Desarrollo 

1. Ingresar a la consola de AWS buscando el servicio Amazon Macie.

<img src="img/ej3-macie-get-start.png"></img>

2. Habilitar Macie, al habilitarlo se genera un rol con la política necesaria para que el servicio acceda al servicio S3.

<img src="img/ej3-habilitar-macie.png"></img>

<img src="img/ej3-habilitar-macie-02.png"></img>

3. Al habilitar el servicio, Macie  da un reporte de los buckets a los que tiene acceso. Habrá que ejecutar un trabajo de escaneo

<img src="img/ej3-macie-dashboard.png"></img>

4. Se debe seleccionar el o los buckets para ser analizados.

<img src="img/ej3-buckets-selected.png"></img>


5. Confirmar el bucket y el costo estimado.

<img src="img/ej3-macie-estimado.png"></img>

6. Para no incurrir en costos periódicos se deberá seleccionar como trabajo único.

<img src="img/ej3-macie-periodicidad.png"></img>

7. En la siguiente pantalla se pueden escoger identificadores personales, son patrones basados en regex o palabras clave que deben ser identificados como información sensible, por defecto Macie ya detecta nombres,direcciones y números de tarjetas de crédito.

<img src="img/ej3-identificadores-personales.png"></img>

8. Se asigna un nombre y descripción para el trabajo.

<img src="img/ej3-macie-add-name-and-description.png"></img>

9. Se revisan los datos configurados, de ser correctos se finaliza la configuración.

<img src="img/ej3-macie-config-review.png"></img>

10. El trabajo comienza a ejecutarse.

<img src="img/ej3-macie-running-job.png"></img>

11.  Completado el trabajo se tendrá acceso a un reporte de hallazgos.

<img src="img/ej3-macie-job-done.png"></img>

12. Verificando el contenido del archivo se puede ver que son 4 nombres encontrados y reportados.

<img src="img/ej3-report-done.png"></img>
