# CatDB

## Overview

The Postgres Cat Database (CatDB) is used to securely store the clinical information of patients at a particular site. In AusCAT, the Oncology Information System (OIS) at each clinic is queried to retrieve the relevant patient cohorts for a particular research project. The clinical features and indicators are stored in the CatDB, while the patient information that can potentially identify them are stored in the Postgres Key Database (KeyDB). Permissions to view and interact with the CatDB are restricted to the site's relevant AusCAT contacts. The patient identifier information is then anonymised in the CatDB which can then be queried downstream for AusCAT-related research projects.

The CatDB component has the following characteristics:

- It is a Postgres relational database.
- The tables contain patients clinical information such as date of birth, gender, histology, dose fractions and timestamps (first fraction date, last fraction date), prescription notes, survival status, smoking history, radiomics, staging etc

## Configuration

Typically, our Docker components are deployed efficiently using a docker-compose file. To store de-identified research data, we have built a custom Docker image for CatDB server.

A template docker-compose service for CatDB in AusCAT is as follows:

```yml
catdb_server:
restart: always
image: 'auscat/catdb_schema:mosaiq'
environment:
    POSTGRES_USER_FILE: /run/secrets/catdb_user
    POSTGRES_PASSWORD_FILE: /run/secrets/catdb_pass
    POSTGRES_HOST: localhost
    PGDATA: /var/lib/postgresql/data/pgdata
    TZ: Australia/Sydney
volumes:
    - cat-pgdata:/var/lib/postgresql/data/
ports:
    - 5434:5432
```
In this example, we have set the tool to run on port 5434 on the host machine and can be accessed via the web broswer (eg. http://localhost:5434), however it can be deployed on any available port. Please confirm that the chosen port is available for use and edit accordingly in the yml configuration.

The environment variable "POSTGRES_USER_FILE" is used in conjunction with "POSTGRES_PASSWORD_FILE" to set a user and its password. This variable will create the specified user with superuser permission and a database with the same name. If it is not specified, then the default user `key_admin` and password `postgres` can be used. "POSTGRES_HOST" is the database host, "PGDATA" refers to the directory that contains postgres data write logs and "TZ" indicates the host machine's timezone.

The Docker image `auscat/catdb_schema:mosaiq` is pulled from AusCAT DockerHub repository.

## Secrets

### Set superuser and password

Under the "Secrets" section in Portainer, create the sensitive variables that require secure tracking for your Docker stack. It is essential to include these secrets prior to deploying the Docker stack. Note that secrets are encrypted during transit and at rest in a Docker Swarm. Outlined below are the required steps -

1. In portainer, from the menu select "Secrets" then click "+ Add secret".
2. Then you need to give the secret a descriptive name and write a definition of the secret in the "Secret" field. Toggle "Encode secret" on if you want to encode the secret (useful when you use a plain-text password).
3. To set the superuser and superuser password for PostgresSQL Cat Database, pass superuser sensitive information of the Key Database into Docker Swarm by creating catdb_user and catdb_pass secrets.

## Docker Image

We currently maintain the `auscat/catdb_schema:tag` image on our AusCAT Dockerhub account for CatDB use in our stack, where the tag can either be mosaiq or aria. An example of this would be `auscat/catdb_schema:mosaiq`. 

It is pulled from the official CatDB [DockerHub](https://hub.docker.com/repository/docker/auscat/catdb_schema/general) repository, developed by AusCAT.

The Dockerfile can be found [here](https://github.com/AustralianCancerDataNetwork/auscat_etl/blob/main/mosaiq/catdb/Dockerfile).

Similar to the other AusCAT Docker components and if you are using Portainer to deploy these components, upgrading the CatDB service is a straightforward process.

1. Access the `Services` page in Portainer.
2. Select the checkbox of the relevant CatDB service, on the left hand side of the table containing the services.
3. Click on `Deploy` and a popup will appear on the screen. Toggle the `Pull latest image` option and then click `deploy'.

This should take a few seconds to redeploy the container with the latest image.