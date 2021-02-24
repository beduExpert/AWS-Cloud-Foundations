# Reto 06 - Creación de un perfil IAM

## Objetivo

* Crear un usuario que no sea el raiz para usar la cuenta

## Desarrollo

>**💡 Nota para experto(a)**
>
>Menciona al alumno la importacia de no usar el usuario raiz en el día a día de la cuenta

### Instrucciones

1. Al regresar a firmarte a tu cuenta AWS, podras observar que existen dos opciones: 
    * Root user
    * IAM user

2. Selecciona `Root user` e ingresa con el email y contraseña que usaste en la creación de la cuenta
<img src="img/1.png"></img>

3. Dentro del buscador de servicios, localiza IAM (Identity and Access Management) e ingresa a `users` ya sea con el menú izquierdo o la opción del dashboard
<img src="img/2.png"></img>
4. Da clic en la opción `Add user`
<img src="img/3.png"></img>
5. Completa la siguiente información
    * nombre de usuario con el que trabajarás en tu día a día
    * otorga aceso programatico (necesario para el CLI y SDK) y a la consola (acceso web)
    * requiere que se cambie el password después del primer ingreso
    * Da click en `Next`
<img src="img/4.png"></img>
6. Procederemos a otorgar los permisos de lo que puede hacer a través de un grupo
    * Da un nombre al grupo como `admin`
    * Seleccionar `AdministratorAccess`
    * Dar click en `Create group`
<img src="img/5.png"></img>
<img src="img/6.png"></img>
7. Agregar usuario al grupo recien creado
<img src="img/7.png"></img>
8. Agregar etiquetas representativas (pregunta a tu instructor qué te sugiere)
9. Descarga el archivo `.csv` que contiene la información de acceso y guardalo en un lugar seguro
<img src="img/8.png"></img>
10. Prueba el acceso con la URL que viene en el archivo `.csv` y cambia tu contraseña