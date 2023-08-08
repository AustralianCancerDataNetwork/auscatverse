# Pentaho

## Overview
[Pentaho](https://www.hitachivantara.com/en-us/products/pentaho-platform/data-integration-analytics.html) is a drag-and-drop Extract, Transform and Load (ETL) Graphical User Interface (GUI) desktop application. It is used in AusCAT to populate the KeyDB and CatDB with relevant information from a centre's Oncology Infomration System (OIS). For example, we can create of complex data population pipelines using the GUI, with little-to-no coding background.

## Dependencies

- Installation of Java 8:

  * Windows: [https://www.openlogic.com/openjdk-downloads](https://www.openlogic.com/openjdk-downloads), select Java 8, Windows, 64bit, JDK. Choose the most recent release. Recommended to set install path in project folder:
  eg. `C:\AusCAT\Java\` or `D:\AusCAT\Java\`

  * Linux: Run the following commands at terminal assuming apt has internet access:
    ```bash
    sudo apt-get update
    sudo apt-get install openjdk-8-jdk
    ```

## Installation

Unlike the rest of the AusCAT stack, Pentaho does not have a dockerised version and must be installed as standalone application.

Follow these steps to install Pentaho on a Windows or Linux flavoured machines:
- [Download](https://sourceforge.net/projects/pentaho/) the latest version of Pentaho's Community Edition: Data Integration
- Unzip to your directory of choice on system (recommend consistent folder structure)
    eg.:
    * Linux: `/auscat/programs/pdi`
    * Windows: `C:\AusCAT\Programs\pdi` or `D:\AusCAT\Programs\pdi`
- Copy jar files to \<pentaho-application-directory\>/lib folder: `CTP.jar`, `util.jar`, `jtds.jar`. These can be found [here]()
- [Download](https://jdbc.postgresql.org/download/) the PostgreSQL JDBC driver for Java 8. Copy the downloaded JDBC jar file to the folder \<pentaho-application-directory\>/lib
- (Optional) Increase heap space
  - Edit spoon.bat (Windows) or spoon.sh (Linux). Increase PENTAHO\_DI\_JAVA\_OPTIONS to increase memory allocation if required.
- Add "PENTAHO\_HOME" as environment variable:
    * Windows: Open control panel -> advanced system settings -> Create new environment variable "PENTAHO\_HOME"="D:\AusCAT\Programs\pdi\" as example.
    * Linux: run the following command in your terminal: ```export PENTAHO_HOME=/auscat/programs/pdi```
- To run:
    * Windows: execute the `spoon.bat` file as an administrator, to enable editing of the pipeline files in Pentaho.
    * Linux: run the following commands with sudo privileges:
    ```bash
    xhost + local: 
    sudo ./spoon.sh 
    ```

## Using Pentaho
Once you have installed Pentaho, you are able to start creating or modifying pipelines to load data into the AusCAT databases. 

### Pipeline structure
Pentaho has 2 main files that are used to build pipelines:
- Job files (*.kjb): these files define what is to be executed within a pipeline. It is made up of ETL transformation steps where each step defines what needs to be excuted at that stage. These steps are linked sequentially with a start and and end step. The 2 main examples of kjb files we have in AusCAT are:
    * Populating the KeyDB end to end
    * Populating the CatDB end to end

- ETL transformations files (*.ktr): these files are saved instances of the ETL transformations that, when combined, make up a job file. Within these files, we define a mini-pipeline made up of atomic steps that relates to a particular part of the population process. The following is an example `ktr` for populating the `Patient` table in the KeyDB (eg. `get_patient_table.ktr`) being made up of atomic steps:
    - asserting the `patient` table exists in the KeyDB
    - getting each unique MRN from the centre's OIS
    - extracting patient information from the OIS for each unique MRN
    - transforming the data fields to be consistent (eg. capitalising names)
    - inserting these patient details along with an anonymised centre-specific patient identifier into the `patient` table


### Configuring a pipeline

#### MOSAIQ/ARIA based centres
We highly recommend that you work off of the template job files we have prepared for MOSAIQ or ARIA based centres. These templates contain template ETL transformations that will need very little manual intervention from you to configure it to run at your AusCAT node. Example things that will/may need updating in the templates:

- Database connections: these are used by Pentaho to connect to different databases used in the pipelines. These are your centre's OIS databases and your AusCAT nodes KeyDB/CatDB. to edit these connections, on the left sidebar there is a "Database Connections" section. Create new database connections for PostgreSQL KeyDB and source OIS databases (either MOSAIQ or ARIA).  Please contact your AusCAT technical contact if you do not have records of your AusCAT node's databases

> Tip: After creating the connections right-click each connection and select "Share" to make this connection available to all ETL transformation files associated with the job.

- Individual steps within ETL transformations: these could be data fields that are referenced in your OIS database when retrieving data from it. For example, we made the assumption that for all MOSAIQ or all ARIA centres, table and column names will be identical. For the most part this is true, but some centres may change this. This will require knowledge of  potential variations may have occured in your centre's OIS and coordination with an AusCAT technical contact to modify the pipelines for your particular centre.


## Troubleshooting

### Cannot save pipeline files when editing in Pentaho
This may be due to a permissions issue where the pipeline files are located in a path that your user does not have write access to.

To solve this, please make sure you run Pentaho as an Administrator/Superuser:
  * For Windows, right click on the `spoon.bat` file and select `Run as Administrator`
  * For Linux based systems, run the following commands:
    ```bash
        xhost + local:
        sudo ./spoon.sh
    ```
