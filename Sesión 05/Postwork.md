# Postwork - Amazon Keyspaces

## 1. Objetivo 
- Administración de datos para la base de datos de AWS compatible con Cassandra; Amazon Keyspaces.

## 2. Requisitos
Credenciales IAM con acceso mediante programación con acceso a AWS Keyspaces, las credenciales deben ser dadas de alta en AWS CLI con el comando `aws configure`

<img src="img/ej2-iam-role-02.png"></img>

Así se puede dar del alta la nueva cuenta de IAM en AWS CLI (especificar las llaves propias)

<img src="img/ej2-new-configure-profile-in-aws-keyspaces.png"></img>

- NoSQL Workbench [instalado](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/workbench.settingup.html).

## 3. Desarrollo 

#### Manos a la obra 👩‍💻

1. Abrir NoSQL Workbench, seleccionar "Launch" en la seccion de Amazon Keyspaces.

<img src="img/ej2-open-nosql-workbench.png"></img>

2. Para este ejercicio, se seleccionará una base de datos pre definida, seleccionar "Credit Card".

<img src="img/ej2-select-credit-card.png"></img>

3. Se podrán observar las tablas de la base de datos de tarjetas de crédito, click en "Visualizer".

<img src="img/ej2-select-visualizer-01.png"></img>

4. Para persistir estas tablas en AWS Keyspaces click en "Commit Amazon Keyspaces".

<img src="img/ej2-commit-keyspaces.png"></img>

5. Descargar el [certificado de conexión](https://www.amazontrust.com/repository/AmazonRootCA1.pem), con el certificado de conexión seleccionar (b) las credenciales IAM (a).

<img src="img/ej2-set-credentials-to-commit.png"></img>

Tardará un par de minutos en generar el espacio de claves y tablas.

<img src="img/ej2-creating-keyspace-and-tables.png"></img>

Para editar datos se hará por medio de la consola de comandos CQL (Cassandra Query Language ).

6. Ir a la consola de AWS Keyspaces.

<img src="img/ej2-access-to-keyspaces.png"></img>

7. Al dar click en tablas se pueden observar las tablas donde se guarda la información, estas tablas ya vienen pre cargadas con información.

<img src="img/ej2-view-tables-keyspaces.png">

8. Click en **Editor CQL**


<img src="img/2.png">


9. Ejecutar el comando `select * from creditcard.transactions_by_creditcard_num` (a,b), CQL es muy similar a SQL de otras bases de datos. Se visualiza un error al ejecutar la consulta (c), al query se debe agregar un `keyspace`, ¿cuál keyspace debo agregar?, click en "Espacios de claves" (c) para descubrirlo.

<img src="img/ej2-execution-select-failed.png"></img>

10. El keyspace para este caso es `creditcard`.

<img src="img/ej2-view-keyspaces.png"></img>

11. Ejecutar el query `select * from creditcard.transactions_by_creditcard_num` agregando el keyspace `creditcard`. Al ejecutar la consulta se pueden ver datos retornados.

<img src="img/ej2-select-data-from-keyspaces-01.png"></img>

## CRUD

Todas las operaciones se pueden realizar en el panel del servicio de AWS Keyspaces en el editor de consultas CQL.

* Generar un nuevo registro sobre la tabla `transactions_by_category`
[Aquí](https://cassandra.apache.org/doc/latest/cql/dml.html#insert) la documentación de CQL para hacer un insert.

* Eliminar un registro sobre la tabla `transactions_by_category`
[Aquí](https://cassandra.apache.org/doc/latest/cql/dml.html#delete) la documentación de CQL para hacer un delete.

* Actualizar un registro sobre la tabla `transactions_by_category`
[Aquí](https://cassandra.apache.org/doc/latest/cql/dml.html#update) la documentación de CQL para hacer un update.

¿Te animas a probar [batch](https://cassandra.apache.org/doc/latest/cql/dml.html#batch)?
