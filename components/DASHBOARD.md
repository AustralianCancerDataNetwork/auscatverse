# Dashboard

This repository contains the source code for a template dashboard project
that can be used within AUSCAT. This template was first developed using Python
(3.8.12), django-plotly-dash (1.6.6), Dash (1.20.0) and Django (3.2.13) web framework.

### Production (User Guide)

### Development

**NOTE**: For images that exist under the premium Dockerhub account, you'll need to login into the Dockerhub account on the local machine, by running `docker login`, then enter the username and password when promted.

**Docker**
The template can be deployed using docker. Via VSCode, you can setup the development containers
using the defined `.devcontainer.json` files and setup the application using its dockerfile-build image. This can be done
by:
1. Making sure that the extension "Remote-Containers" is installed.
2. Clicking on the bottom left corner of VSCode.
3. Select the option "Open folder in container". This will then open a VSCode instance of the container and map it to your local repository.
Development and launching of the dashboard can now occur in this new VSCode window.

Alternatively, you can spin up the containers by running the command:
```bash
docker-compose up -d
```

### Head and Neck (template and playground dashboard)
A dummy dashboard that can be used as a template and playground for testing dashboard designs. It visualises and reports patient
data avaliability at a AusCAT node, using the TCIA Head and Neck cancer PET/CT dataset [[1]](#1) as a mockup of its real data.

#### Intializing the head and neck dashboard database

```bash
python manage.py migrate
```

1.  For the template Head and Neck dashboard, we assume that another container is running a simluated AusCAT CAT_DB database
    (it will mostly likely an instance of the auscat_installation image)

2. Since this is a locally-run database (for development purposes), the data can be imported into the database by running the script data_availability/data/import_csv_into_postgres.py. WARNING: Please make sure this scriptdoes not run a function named "create_table" unless you are absolutely sure you'd like to truncate the medical table in your database. By default this does not run, but please be aware before running this script

3. To initialise the Django local DB, run the command:

    ```bash
    python manage.py migrate
    ```

4. Create a super user for this development dashboard by running the command and selecting the appropriate options:

    ```bash
    python manage.py createsuperuser
    ```

5.  The connection configuration to the database is controlled through the admin panel interface. Utlimately, this 
    can be defined to point to real AusCAT databases for actual dashboard implementations. By default, this is not set.
    To set this, run the following command, navigate to http://localhost:8080/admin and login as a super user:

    ```bash
    python manage.py runserver
    ```

Once you have access to the admin panel, under the "Head and Neck" application, select Config, create a new Config and in here you
can add the appropriate postgres database credentials (either local for development or a live AusCAT postgres DB).

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

### Running the application

```bash
python manage.py runserver
```

This will start the django server and launch the app. you can access the application using
[http://localhost:8000](http://localhost:8000)

### Non-Docker approach
**Non-Docker approach**: This template can be ran without docker, by following these steps:

1. Install the poetry [tool](https://python-poetry.org/docs/).
2. Create a python virtual environment and activate it.
3. Navigate to `dashboard/` and run:

```bash
poetry install
```
This will install the python packages into your virtual environment




## References
<a id="1">[1]</a> 
Vallières, M. et al.
Radiomics strategies for risk assessment of tumour failure in head-and-neck cancer.
Sci Rep 7, 10117 (2017).
doi: 10.1038/s41598-017-10371-5