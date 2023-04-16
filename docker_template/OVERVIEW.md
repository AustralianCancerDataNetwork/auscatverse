# Overview

In the AusCAT federated learning network for radiation oncology, we have utilised Docker as a tool to containerise the required software into flexible submodules that are isolated from a host machine’s operating system.
</br>
</br>
This included databases and structured query language (SQL) extraction tools (PostgreSQL, pgAdmin, Pentaho Data Integration), DICOM-RT anonymisation and PACS (Clinical Trials Processor (CTP), Orthanc), data harmonisation (D2RQ Platform), and the AusCAT federated learning platform (Apache Tomcat).
</br>
</br>
A Docker compose script orchestrates the tools and additional scripts were implemented to configure each environment as well as an automated testing pipeline for each component.