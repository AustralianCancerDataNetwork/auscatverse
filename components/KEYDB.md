# KeyDB

## Overview

The Postgres Key Database (KeyDB) is used to securely store the identifying information of patients at a particular site. In AusCAT, the Oncology Information System (OIS) at each clinic is queried to retrieve the relevant patient cohorts for a particular research project. Patient information that can potentially identify them are stored in the KeyDB, while the clinical features and indicators are stored in the Postgres Cat Database (CatDB). Permissions to view and interact with the KeyDB are restricted to the site's relevant AusCAT contacts. The patient identifier information is then anonymised in the CatDB which can then be queried downstream for AusCAT-related research projects. The KeyDB holds the key anonymisation linkage ID's that can be forward-tracked to the de-identified patients in the CatDB, if need be. 

The KeyDB component has the following characteristics:

- It is a Postgres relational database.
- The tables contain information regarding patients demographics/identifiers cohorts, DICOM Identifiers, linkage information, etc. The schemas can be slightly different across the centres.
- The Clinical Trial Processor (CTP) communicates with KeyDB to anonymise/de-identify patient data.

## Configuration

Typically, our Docker components are deployed efficiently using a docker-compose file. To standardise the patient anonymisation handling, we have built a custom Docker image for KeyDB server which contains a standardised schema for storing patient identifiers and linking it with de-identifiers, which allows the stack to be deployed across different sites that may use different OIS and Picture Archiving and Communication System (PACS).

A template docker-compose service for KeyDB in AusCAT is as follows:

```yml
version: '3.8'
services:
   keydb_server:
      # Postgres Key database service
      restart: always
      # Link to the image source from DockerHub
      image: 'auscat/keydb_schema:latest'
      ports:
         - 5433:5432
      environment:
         POSTGRES_USER_FILE: /run/secrets/keydb_user 
         POSTGRES_PASSWORD_FILE: /run/secrets/keydb_pass
         POSTGRES_HOST: localhost 
         PGDATA: /var/lib/postgresql/data/pgdata 
         TZ: Australia/Sydney 
      # Postgres data storage location (as a volume)
      volumes:
        - key-pgdata:/var/lib/postgresql/data/ 
      secrets:
        - keydb_user # Add Secret on Portainer 
        - keydb_pass # Add Secret on Portainer 
        - keydb_ctp_pass # Add Secret on Portainer (ctp_key_db_select's password)

secrets:
  keydb_user:
    external: true
  keydb_pass:
    external: true
  keydb_ctp_pass:
    external: true
    file: /run/secrets/keydb_ctp_pass

volumes:
  key-pgdata:
```
In this example, we have set the tool to run on port 5433 on the host machine and can be accessed via the web broswer (eg. http://localhost:5433), however it can be deployed on any available port. Please confirm that the chosen port is available for use and edit accordingly in the yml configuration.

The environment variable "POSTGRES_USER_FILE" is used in conjunction with "POSTGRES_PASSWORD_FILE" to set a user and its password. This variable will create the specified user with superuser permission and a database with the same name. If it is not specified, then the default user `key_admin` and password `postgres` can be used. "POSTGRES_HOST" is the database host, "PGDATA" refers to the directory that contains postgres data write logs and "TZ" indicates the host machine's timezone.

The Docker image `auscat/keydb_schema:latest` is pulled from AusCAT DockerHub repository.

## Secrets

### Set superuser and password

Under the "Secrets" section in Portainer, create the sensitive variables that require secure tracking for your Docker stack. It is essential to include these secrets prior to deploying the Docker stack. Note that secrets are encrypted during transit and at rest in a Docker Swarm. Outlined below are the required steps -

1. From the menu, select `Secrets` and click `+ Add secret`.
2. In the `Secret` field, you need to provide a descriptive name and write a definition of the secret.
3. Toggle `Encode secret` on if you want to encode the secret (useful when you use a plain-text password).
4. To set the superuser and superuser password for PostgresSQL Key Database and for anonymisation, pass superuser sensitive information of the Key Database and CTP password into Docker Swarm by creating the following secrets:
    - **keydb_user** (default: key_admin)
    - **keydb_pass** (default: postgres)
    - **keydb_ctp_pass** (default: changeme)
 

## Docker Image

We currently maintain the `auscat/keydb_schema:latest` image on our AusCAT Dockerhub account for KeyDB use in our stack, with the tag `latest` being the most up-to-date version. 

It is pulled from the official KeyDB [DockerHub](https://hub.docker.com/repository/docker/auscat/keydb_schema/general) repository, developed by AusCAT.

The Dockerfile can be found [here](https://github.com/AustralianCancerDataNetwork/auscat_etl/blob/main/mosaiq/keydb/Dockerfile).

Similar to the other AusCAT Docker components and if you are using Portainer to deploy these components, upgrading the KeyDB service is a straightforward process.

1. Access the `Services` page in Portainer.
2. Select the checkbox of the relevant KeyDB service, on the left hand side of the table containing the services.
3. Click on `Deploy` and a popup will appear on the screen. Toggle the `Pull latest image` option and then click `deploy'.

This should take a few seconds to redeploy the container with the latest image.
