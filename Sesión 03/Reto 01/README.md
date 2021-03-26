# Reto 1

## 1. Objetivo 
- Agregar Security Groups.

## 2. Requisitos 
- Acceso a AWS Console.
- Haber completado el `Ejercicio 3`

## 3. Desarrollo

Se configurarán los grupos de seguridad que darán acceso al tráfico de red a las instancias EC2 y las instancias RDS.

1. Seleccionar **Security Groups**.

![pw1-security-groups-access.png](img/pw1-security-groups-access.png)

2. Click en **Create Security Group**.

![pw1-create-security-group.png](img/pw1-create-security-group.png)

3. Configurar el grupo de seguridad como:

  **a., b.** Establecer un nombre descriptivo así como una descripción

  **c.** Seleccionar la red VPC a la cual pertenecerá la regla de seguridad.

  **d., e..** Establecer el tipo de tráfico permitido de entrada hacia AWS desde una fuente (source).

  **f., g.** Establecer el tráfico que se permitirá de salida y hacia donde se podrá conectar desde AWS, se recomienda dejar abierto el tráfico por temas de actualizaciones y descarga de software.

![pw1-add-security-group-web.png](img/pw1-add-security-group-web.png)

![pw-seg-group-web-01-done.png](img/pw-seg-group-web-01-done.png)


4. Generar un nuevo grupo de seguridad para el tráfico de base de datos de Postgres.

  **a., b.** Establecer un nombre descriptivo así como una descripción del grupo de seguridad.

  **c.** Seleccionar la red VPC a la cual pertenecerá la regla de seguridad.

  **d., e.** Establecer el tipo de tráfico permitido de entrada hacia AWS desde una fuente (source), para este caso se permitirá acceso de Postgres (puerto 5432) desde cualquier origen.

  **f., g.** Establecer el tráfico que se permitirá de salida y hacia donde se podrá conectar desde AWS, se recomienda dejar abierto el tráfico por temas de actualizaciones y descarga de software.
![pw1-seg-group-postgres-01.png](img/pw1-seg-group-postgres-01.png)

![pw-seg-group-db-01-done.png](img/pw-seg-group-db-01-done.png)


5. Generar un nuevo grupo de seguridad para el tráfico de conexión SSH.

  **a., b.** Establecer un nombre descriptivo así como una descripción del grupo de seguridad.

  **c.** Seleccionar la red VPC a la cual pertenecerá la regla de seguridad.

  **d., e.** Establecer el tipo de tráfico permitido de entrada hacia AWS desde una fuente (source), para este caso se permitirá acceso SSH (puerto 22) desde cualquier origen.

  **f., g.** Establecer el tráfico que se permitirá de salida y hacia donde se podrá conectar desde AWS, se recomienda dejar abierto el tráfico por temas de actualizaciones y descarga de software.
![pw1-add-seg-gr-ssh-01.png](img/pw1-add-seg-gr-ssh-01.png)
