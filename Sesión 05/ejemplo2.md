# Ejemplo 2

# 1. Objetivo 🎯
- Administración de datos para la base de datos de AWS compatible con Cassandra; Amazon Keyspaces.

# 2. Requisitos 📌
Credenciales IAM con acceso mediante programación con acceso a AWS Keyspaces, las credenciales deben ser dadas de alta en AWS CLI con el comando `aws configure`
![ej2-iam-role-02.png](ej2-iam-role-02.png)

Así se puede dar del alta la nueva cuenta de IAM en AWS CLI (especificar las llaves propias)
![ej2-new-configure-profile-in-aws.png](ej2-new-configure-profile-in-aws-keyspaces.png)


- NoSQL Workbench [instalado](https://docs.aws.amazon.com/amazondynamodb/latest/developerguide/workbench.settingup.html).

# 3. Desarrollo 📑

1. Abrir NoSQL Workbench, seleccionar "Launch" en la seccion de Amazon Keyspaces.
![ej2-open-nosql-workbench.png](ej2-open-nosql-workbench.png)

2. Para este ejercicio, se seleccionará una base de datos pre definida, seleccionar "Credit Card".
![ej2-select-credit-card.png](ej2-select-credit-card.png)

3. Se podrán observar las tablas de la base de datos de tarjetas de crédito, click en "Visualizer".
![ej2-select-visualizer-01.png](ej2-select-visualizer-01.png)

4. Para persistir estas tablas en AWS Keyspaces click en "Commit Amazon Keyspaces".
![ej2-commit-keyspaces.png](ej2-commit-keyspaces.png)

5. Descargar el [certificado de conexión](https://www.amazontrust.com/repository/AmazonRootCA1.pem), con el certificado de conexión seleccionar (b) las credenciales IAM (a).
![ej2-set-credentials-to-commit.png](ej2-set-credentials-to-commit.png)

Tardará un par de minutos en generar el espacio de claves y tablas.
![ej2-creating-keyspace-and-tables.png](ej2-creating-keyspace-and-tables.png)

Para editar datos se hará por medio de la consola de comandos CQL (Cassandra Query Language ).

6. Ir a la consola de AWS Keyspaces.
![ej2-access-to-keyspaces.png](ej2-access-to-keyspaces.png)

7. Al dar click en tablas se pueden observar las tablas donde se guarda la información, estas tablas ya vienen pre cargadas con información.
![ej2-view-tables-keyspaces.png](ej2-view-tables-keyspaces.png)

8. Click en "Editor CQL"
![ej2-goto-editor-sql.png](ej2-goto-editor-sql.png)

9. Ejecutar el comando `select * from creditcard.transactions_by_creditcard_num` (a,b), CQL es muy similar a SQL de otras bases de datos. Se visualiza un error al ejecutar la consulta (c), al query se debe agregar un `keyspace`, ¿cuál keyspace debo agregar?, click en "Espacios de claves" (c) para descubrirlo.
![ej2-execution-select-failed.png](ej2-execution-select-failed.png)

10. El keyspace para este caso es `creditcard`.
![ej2-view-keyspaces.png](ej2-view-keyspaces.png)

11. Ejecutar el query `select * from creditcard.transactions_by_creditcard_num` agregando el keyspace `creditcard`. Al ejecutar la consulta se pueden ver datos retornados.
![ej2-select-data-from-keyspaces-01.png](ej2-select-data-from-keyspaces-01.png)
