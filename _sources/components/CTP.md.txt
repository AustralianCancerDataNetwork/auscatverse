# Clinical Trial Processor

## Overview

The [clinical trial processor (CTP)](https://github.com/johnperry/CTP) is an open-source Java-based package to pass DICOM imaging files through a pipeline of automated processes. For AusCAT it is used to deidentify the metadata for DICOM imaging files extracted from clinical databases prior to performing research studies.

CTP implements a series of pipelines, typically consisting of a DICOM import service which listens on a specified port (default: 8104) for files transmitted via DICOM protocol, followed by an anonymisation or lookup script for modifying the imported DICOM metadata and then a DICOM export service to transmit the processed DICOM files to a DICOM PACS (Picture Archiving and Communication System) with a default location as Orthanc on port 4242 (see Docker image for [Orthanc in AusCAT](https://github.com/AustralianCancerDataNetwork/auscatverse/blob/main/components/ORTHANC.md))). The original CTP package and examples has built-in functionality of an anonmyisation script using hash functions on the metadata tags. By incorporating plugins additional functions for database lookup for deidentification can be implemented.

For each of these internal CTP services there is a temporary folder with processing queue to handle higher data loads and a quarantine folder where files that were unable to be processed are stored for further examination. All of this can be monitored on a web page interface where CTP displays the status, queue, quarantine, configuration and scripts (default port: 9090).

Two containerised versions are available on this AusCAT-based repository, one with the default CTP pipeline and one with a customised pipeline containing a database lookup implementation which is used in the AusCAT infrastructure. The customized configuration depends on a PostgreSQL database (KeyDB) to be available with patient identifiers matching the DICOM files to be deidentified.

The CTP depends on a queryable KeyDB and destination PACS which in AusCAT has been [Orthanc PACS](https://github.com/AustralianCancerDataNetwork/auscatverse/blob/main/components/ORTHANC.md).

## Configuration

The CTP service is built and deployed via a docker-compose file. A template to be inserted in a docker-compose script is given below.

```yaml
version: '3.8'
services:
 ctp_customized:
   image: "auscat/customized_ctp:latest"
   depends on:
   - keydb_server
   ports:
   - 9090:9090
   - 8104:8104
   environment:
    ORTHANC_HOSTNAME: orthanc.hostname.address.url
    ORTHANC_PORT: 4242 # 4242 is the default that is being used in the auscat/orthanc:latest image
   volumes:
   - ctp-logs:/logs
   - ctp-customized:/ctp_temp
   secrets:
      - keydb_user      # add Secret on Portainer (a KeyDB PostgreSQL user username)
      - keydb_pass      # add Secret on Portainer (a KeyDB PostgreSQL user password)
      - ctp_admin_pass  # add Secret on Portainer (admin's password for CTP web GUI)

secrets:
  keydb_user:
    external: true
  keydb_pass:
    external: true
  ctp_admin_pass:
    external: true

volumes:
 ctp-customized:
 ctp-logs:
 
```

Port 9090 is used for the web page interface and port 8104 is the chosen port for DICOM communications. When sending DICOM data via the DICOM standard protocol use port 8104.

The volumes for `logs` and `ctp_temp` are used for traceability and debugging. Logs are recommended viewing when DICOM files are not passing through the anonymizer. It is recommended to store the volume linked to ctp_temp where there is sufficient disk space. If CTP is a bottleneck ctp_temp will grow siginificantly in size when performing large batch data exports.

Once CTP is running you can log into the web interface (running on port 9090 by default) using the username `admin` and the password specified in the `ctp_admin_pass` secret. By default a password of `admin` is used, but this should overriden using a secret in a production environment.

## Environment Variables

The following must be configured for the CTP Docker service to run:
- `ORTHANC_HOSTNAME`: this must contain the hostname of the **VM** that is running the AusCAT Orthanc Docker service. This allows CTP to move the anonoymised DICOM objects to your intended Orthanc server.
- `ORTHANC_PORT`: this must contain the DICOM port that the AusCAT Orthanc service is listening on for the hostname name mentioned above.

> Note: if you're running the Orthanc Server in a Docker container, please make sure to refer to the VM hostname and the VM port it is listening on for both the `ORTHANC_HOSTNAME` and `ORTHANC_PORT` respectively (and not to the internal Docker service name and container port!). If you are unsure, please refer to the Orthanc [documentation](./ORTHANC.md) for more information.

## Secrets

The docker image requires the following secrets to be set in Portainer for CTP to operate:

- keydb_user: A KeyDB user username used for authenticating against the KeyDB
- keydb_pass: A KeyDB user password for the above username, used for authenticating against the KeyDB
- ctp_admin_pass: The password the admin user should use to login to the CTP web interface.

## Docker images and template docker-compose

For CTP two docker images are maintained within repository auscat_imaging and on the project DockerHub repository. These are:
- `auscat/customized_ctp:latest`: this is the main image that is being used currently in AusCAT, as it contains additional dyanmic functionaility for handling configuration issues. Its Dockerfile is contained [here](https://github.com/AustralianCancerDataNetwork/auscat_imaging/blob/main/ctp-customized/Dockerfile).
- `auscat/default_ctp:latest`: this is the initial image that was being used in AusCAT. This will most likely be archived in the near future, but for now ignore this image (unless you are interested in using a default version of the CTP program, that has no other additional functionaility described above). Its Dockerfile is contained [here](https://github.com/AustralianCancerDataNetwork/auscat_imaging/blob/main/ctp-default/Dockerfile).
