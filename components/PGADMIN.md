# PGAdmin

## Overview

[PGAdmin](https://www.pgadmin.org/) is a Graphical User Interface (GUI) web-based tool that can be used to interact with Postgres databases. It allows for viewing and inspecting what structure and data is contained within them. Using the in-built account management system, the tool can be configured to only grant access to users who have the required ethics permissions to access the database

This tool is great for the following scenarios:
- To get a quick grasp of what AusCAT data is available at your centre
- Run quick queries/SQL reports on your AusCAT data
- Quickly interact with the database in the case of development or testing, eg.:
   - Purging/flushing or quickly restructuring the databases 
   - Entering dummy data
   - Integrating new features using the simulation environment databases

This component requires the AusCAT databases (KeyDB and CatDB) to be deployed along with it, in order to operate correctly by default. More information on the configuration of PGAdmin can be in the next section. 

## Configuration

Typically, our Docker components are deployed efficiently using a docker-compose file. To further simplify setting up user accounts and server connections within the tool, we have built a custom Docker image for PGAdmin which handles these things, allowing the user to access the tool with a default admin user who can further modify user access and server connection setups.

A template docker-compose service for PGAdmin in AusCAT is as follows:

```yml
   pgadmin:
      restart: always
      image: 'auscat/pgadmin4'
      ports: 
         - 5050:80
      environment:
         PGADMIN_DEFAULT_EMAIL: john.smith@org.com
         PGADMIN_DEFAULT_PASSWORD: a_hopefully_complex_password
```
In this example, we have set the tool to run on port 5050 on the host machine, and can be accessed via the web broswer (eg. http://localhost:5050), however it can be deployed on any available port. Please confirm that the chosen port is available for use and edit accordingly in the yml configuration. 

The 2 environment variables "PGADMIN_DEFAULT_EMAIL" and "PGADMIN_DEFAULT_PASSWORD" are used to allow a default admin user access to the tool.

The Docker image `auscat/pgadmin4` is built using predefined configuration files, these are:
- [servers.json](https://github.com/AustralianCancerDataNetwork/auscat_installation/blob/main/pgadmin/servers.json): this provides pgadmin in the docker image with the appropriate connection strings to connect to the local KEY_DB and CAT_DB databases deployed in the docker stack. If you would like to define your own json file for more specific "out-of-the-box" configuration, you can follow the section below.
- [pgpass](https://github.com/AustralianCancerDataNetwork/auscat_installation/blob/main/pgadmin/pgpass): This defines some placeholder user logins to access the databases in PGAdmin. These are overwritten by secrets created in Portainer. Information on the secrets can be found in the below section.

## Secrets
By default, the image requires 4 secrets to be set in portainer for the logging access to work, these are:
- keydb_user: The username for the KEY_DB database server
- keydb_pass: The password for the KEY_DB database server username indicated above
- catdb_user: The username for the CAT_DB database server
- catdb_pass: The password for the CAT_DB database server username indicated above

Once these have been set, you are ready to deploy the service, either in isolation or together with the other AusCAT components.

## Docker Image

We currently maintain the `auscat/pgadmin4` image on our AusCAT dockerhub account for PGAdmin use in our stack, with the tag `latest` being the most up-to-date version.

It is built on top of the [offical PGAdmin image](https://hub.docker.com/r/dpage/pgadmin4/) developed by dpage.

The Dockerfile can be found [here](https://github.com/AustralianCancerDataNetwork/auscat_installation/blob/main/pgadmin/Dockerfile).

Similar to the other AusCAT Docker components and if you are using Portainer to deploy these components, upgrading the PGAdmin service is a straightforward process.
1. Access the "Services" page in portainer
2. Select the checkbox of the relevant PGAdmin service, on the left hand side of the table containing the services.
3. Click on "Deploy" and a popup will appear on the screen. Toggle the "Pull latest image" option and then click deploy.

This should take a few seconds to redeploy the container with the latest image.


## Contributions

The following is a guide on how to build your own version of the PGAdmin tool for use in AusCAT:

1. The `servers.json` file can be modified for you own use case, here is a description of what the options for each server is used for:
   - Name: Name of the database to connect to in PGAdmin. This is merely a descriptive name for your benefit
   - Group: The entity under which this database should be stored for PGAdmin's inner workings. This should always be set to "Servers"
   - Port: The port on which the database server is running on the "Host" you are trying to connect to. Be default, this is 5432 as we use the docker service hostname (either "keydb_server" or "catdb_server") and this port is (nearly) always used for hosting the postgres servers on, depending on your stack deployment. 
   - Host: This is the hostname where the database server is running on. Again, by default this is set to either "keydb_server" or "catdb_server" but can also point to other hostnames depedning on your setup or potentially if the postgres servers run on a different VM/machines than PGAdmin.
   - Username: the username to access the database server. We have defaults set for the CAT_DB ("catdb_user"), but again depending on how you configure your CAT_DB this will most likely change, and may be of benefit to change this as well.
   - PassFile: the path to the file that will contain credentials to connect to the database. The default option is "/pgpass". We suggest to keep this option the same as the default, but to change the pgpass file that will be copied into the image when rebuilding it.
   - Comment: A description of what this connection is. This can be set what you consider helpful to identify this connection in PGAdmin.
   
   The remaining options can be left as default:
   
   - MaintenanceDB: "postgres"
   - Timeout: 60
   - UseSSHTunnel: 0
   - TunnelPort: 22
   - SSLMode: "prefer"
   - TunnelAuthentication: 0
   - KerberosAuthentication: false

2. To integrate the `server.json` and `pgpass` files with the docker building process, a [custom entrypoint file](https://github.com/AustralianCancerDataNetwork/auscat_installation/blob/main/pgadmin/custom_entrypoint.sh) is used. This injects the new connection strings and dynamic Portainer credential secrets into the PGAdmin config files. If further customisation is required to config files, they should be dealt with in a similar custom entrypoint file.

3. To build the new docker image, you can use the command `docker build -t auscat/pgamin4:YOUR_TAG`, where `YOUR_TAG` is your specific tag of the image.

4. (Recommended) Once the image has been built, attempt to test it out in the simulation environment setting with some public data in both the KEY_DB and CAT_DB.

5. (Optional) If you wish to share these images with the broader AusCAT, we recommend that you clone the `auscat_installation` repository, create a new local branch and commit/push your new changes to this branch. Finally, create a Pull request into the "main" branch with appropriate details on what has been added/changed.

Once a member of the AusCAT technical team has reviewed the PR and indicated approval, then a push to the AusCAT Dockerhub account may occur for others to use.
