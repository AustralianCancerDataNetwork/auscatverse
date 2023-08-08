# Pentaho Guide

## Prerequisite Java installation

- Installation of Java 8:
  - Windows: [https://www.openlogic.com/openjdk-downloads](https://www.openlogic.com/openjdk-downloads), select Java 8, Windows, 64bit, JDK. Choose the most recent release. Recommended to set install path in project folder \<C/D\>:\AusCAT\Java\
  - Linux: Run the following commands at terminal assuming apt has internet access.
 sudo apt-get update
 sudo apt-get install openjdk-8-jdk

## Installing Pentaho

- [Download](https://sourceforge.net/projects/pentaho/) latest version of Pentaho's Community Edition: Data Integration
- Unzip to your directory of choice on system (recommend consistent folder structure)
  - Linux: /auscat/programs/pdi
  - Windows: \<C/D\>:\AusCAT\Programs\pdi\
- Copy jar files to \<pentaho-application-directory\>/lib folder: CTP.jar, util.jar, jtds.jar
- [Download](https://jdbc.postgresql.org/download/) the PostgreSQL JDBC driver for Java 8. Copy the downloaded JDBC jar file to the folder \<pentaho-application-directory\>/lib
- (Optional) Increase heap space
  - Edit spoon.bat (Windows) or spoon.sh (Linux). Increase PENTAHO\_DI\_JAVA\_OPTIONS to increase memory allocation if required.
- Add "PENTAHO\_HOME" as environment variable
  - Windows: Open control panel  advanced system settings  Create new environment variable "PENTAHO\_HOME"="D:\AusCAT\Programs\pdi\" as example.
- To run:
  - Execute spoon.bat in Windows or spoon.sh in Linux.

## Guide to templates

Job files (\*.KJB) coordinate sequence of ETL transformations (\*.KTR) files.

For the KeyDB there is a coordinating LoadKeyDB.kjb file.

- Open the file and in the sidebar there are database connections. Create new database connections for PostgreSQL KeyDB and source OIS databases (either MOSAIQ or ARIA).


- After creating the connections right-click each connection and select "Share" to make this connection available to all ETL transformation files associated with the job.