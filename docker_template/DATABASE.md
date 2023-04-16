# Databases
## Key Database
The Postgres Key Database is used to store patient identifiers and data required for linkage.

### Docker Image 
Pull the image **auscat/keydb_schema** from AusCAT DockerHub repository.

### Manage sensitive data with Docker secrets
#### Set superuser and password
In portainer, under the “Secrets” section, create the sensitive variables that you would like to track securely for your docker stack. You need to add secrets before deploying the docker stack, secrets are encrypted during transit and at rest in a Docker Swarm:
</br>
1. From the menu select "Secrets" then click " + Add secret". 
2. Then you need to give the secret a descriptive name and write a definition of the secret in the "Secret" field. 3. Toggle "Encode secret" on if you want to encode the secret (useful when you use a plain-text password).
4. To set the superuser and superuser password for PostgresSQL Key Database, pass superuser sensitive information of the Key Database into Docker Swarm by creating **keydb_user** and **keydb_pass** secrets. 
</br>
</br>

## Cat Database
The Postgres Cat Database is used to store de-identified research data.

### Docker Image 
Pull the image **auscat/catdb_schema:tag** from AusCAT DockerHub repository, where the tag can either be ```mosaiq``` or ```aria```. An example of this would be ```auscat/catdb_schema:mosaiq```.

### Manage sensitive data with Docker secrets
#### Set superuser and password
1. In portainer, from the menu select "Secrets" then click " + Add secret". 
2. Then you need to give the secret a descriptive name and write a definition of the secret in the "Secret" field. 3. Toggle "Encode secret" on if you want to encode the secret (useful when you use a plain-text password).
4. To set the superuser and superuser password for PostgresSQL Cat Database, pass superuser sensitive information of the Key Database into Docker Swarm by creating **catdb_user** and **catdb_pass** secrets. 

## pgAdmin
pgAdmin 4 is a web based administration tool for the PostgreSQL database. It provides a graphical user interface (GUI) to manage and administer PostgreSQL databases and servers. With pgAdmin, users can create and manage databases, execute SQL queries, monitor server activity and performance, manage database objects such as tables, views, and indexes, and more. </br>
In the AusCAT, the PGAdmin tool lets you explore the tabular data in the CatDB and KeyDB databases. </br>
Visit http://[IP_ADDRESS]:5050 in your web browser. 

### Docker Image 
To manage Key Database, use the Docker image **dpage/pgadmin4**.</br>
To manage Cat Database, use the Docker image **auscat/pgadmin4**.

### Configuration 
In order to create an initial administrator account to access pgAdmin, it is necessary to set up environment variables during launch. Both of these variables are mandatory and must be configured during the launch process:
* **PGADMIN_DEFAULT_EMAIL**
* **PGADMIN_DEFAULT_PASSWORD**