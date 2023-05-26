# Dashboard

## Overview
The dashboard developed using Python (version 3.8.12), django-plotly-dash (version 1.6.6), Dash (version 1.20.0), and Django (version 3.2.13) web framework aims to provide data visualization interfaces. It offers a user-friendly interface that enables users to input custom queries through the interface and explore patient data availability at an AusCAT node. 

### Usage
1.  For the [template](https://github.com/AustralianCancerDataNetwork/auscat_etl/tree/main/dashboard) Head and Neck dashboard, we assume that another container is running a simluated AusCAT CAT_DB database (it will mostly likely an instance of the auscat_installation image)

2. Since this is a locally-run database (for development purposes), the data can be imported into the database by running the script data_availability/data/import_csv_into_postgres.py. WARNING: Please make sure this scriptdoes not run a function named "create_table" unless you are absolutely sure you'd like to truncate the medical table in your database. By default this does not run, but please be aware before running this script

3. To initialise the Django local DB, run the command:
    ```bash
    python manage.py migrate
    ```

4. Create a super user for this development dashboard by running the command and selecting the appropriate options:
    ```bash
    python manage.py createsuperuser
    ```

5. Run the application by using the command:
    ```bash
    python manage.py runserver
    ```
    This will start the django server and launch the app. you can access the application using [http://localhost:8000](http://localhost:8000)
### Dependencies
#### Head and Neck (template and playground dashboard)
A dummy dashboard that can be used as a template and playground for testing dashboard designs. It visualises and reports patient data avaliability at a AusCAT node, using the TCIA Head and Neck cancer PET/CT dataset [[1]](#1) as a mockup of its real data.

### Ports and network access
8000 <br>
Port 8000 provides access to the web interface of the dashboard.

## Configuration 
To set the connection configuration to the database by navigating to http://localhost:8000/admin and login as a super user. <br>

The connection configuration to the database is controlled through the admin panel interface. Utlimately, this can be defined to point to real AusCAT databases for actual dashboard implementations. By default, this is not set. <br>

Once you have access to the admin panel, under the "DATA_AVALIBILITY" application, select Config, create a new Config and in here you can add the appropriate postgres database credentials (either local for development or a live AusCAT postgres DB). <br>

*NOTE*: For now, when using the local cat_db database deployed through docker, the following are the usual credentials:
    
    ```json
    {
        "Db name": "cat_db",
        "Db username": "postgres",
        "Db password": "postgres",
        "Db servername": "127.0.0.1",
        "Db port": "5434"
    }
    ```

### Docker Image
**NOTE**: For images that exist under the premium Dockerhub account, you'll need to login into the Dockerhub account on the local machine, by running `docker login`, then enter the username and password when promted.

AusCAT's Orthanc image is available at: ```auscat/data_availability_dashboard``` <br>
The AusCAT Docker image Dockerfile is available at: https://github.com/AustralianCancerDataNetwork/auscat_etl/blob/main/dashboard/Dockerfile <br>
Which can be built using the command: 
```
cd auscat_etl/dashboard
docker-compose up -d
```

**Developer Guide**:
Alternatively, via VSCode, you can setup the development containers using the defined `.devcontainer.json` files and setup the application using its dockerfile-build image. This can be done by:
1. Making sure that the extension "Remote-Containers" is installed.
2. Clicking on the bottom left corner of VSCode.
3. Select the option "Open folder in container". This will then open a VSCode instance of the container and map it to your local repository. Development and launching of the dashboard can now occur in this new VSCode window.

### Non-Docker approach
**Non-Docker approach**: The [template](https://github.com/AustralianCancerDataNetwork/auscat_etl/tree/main/dashboard) can be ran without Docker, by following these steps:

1. Install the poetry [tool](https://python-poetry.org/docs/).
2. Create a python virtual environment and activate it.
3. Navigate to `dashboard/` and run:

```bash
poetry install
```
This will install the python packages into your virtual environment.

## Troubleshooting
### Logs
The dashboard container will log to the standard output as is usual with Docker. These logs can be inspected within either Portainer or terminal to determine if dashboard is reporting any errors.

## References
<a id="1">[1]</a> 
Vallières, M. et al. Radiomics strategies for risk assessment of tumour failure in head-and-neck cancer.
Sci Rep 7, 10117 (2017). doi: 10.1038/s41598-017-10371-5

