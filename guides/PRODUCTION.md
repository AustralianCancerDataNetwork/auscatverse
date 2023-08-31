# Production Deployment

This guide outlines the steps required to deploy an AusCAT node in a production setting (i.e. real patient data will be loaded in and anonymised in this environment). This guide assumes you have worked through the steps involved in setting up the [Simulation Environment](../simulation/SIMULATION.md). This document provides guidance on additional considerations when deploying this in a production setting.

> Important: Throughout this process, you will be required to define secure passwords for various components of this AusCAT system. It is essential that choose secure passwords in a production environment! Consider using a [password generator](https://my.norton.com/extspa/passwordmanager?path=pwd-gen) to generate secure passwords containing at least 16 characters.

## Infrastructure

Each AusCAT production server should be installed on dedicated hardware located within a hospital/organisation or on a cloud service which has been configured by your IT department to be only accessible via your local corporate network. For a production system, two separate Virtual Machines (VM) are recommended. This guide assumes that two VMs with the following configuration are available for deployment:

### VM 1: KeyVM

This Virtual Machine will host the [KeyDB](../components/KEYDB.md) and related components which store and process **identifiable** patient data.

- Ubuntu 22.04 Operating System
- 2 vCPUs
- 8GB RAM
- 256GB Disk

### VM 2: CatVM

This Virtual Machine will host the [CatDB](../components/CATDB.md) and related components which store and process **anonymised** patient data.

- Ubuntu 22.04 Operating System
- 4 vCPUs
- 16GB RAM
- 256GB disk to host operating system
- 4TB disk to store imaging data (this amount may vary depending on amount of imaging data available)

> Note: While Docker can be deployed and run on Windows using Windows Subsystem for Linux, we currently do not have any documentation on installing AusCAT on this platform.

### VM Setup

Ensure the following is setup and installed on **both** VMs prior to proceeding.

#### Docker & Portainer

Ensure the [Docker and Portainer](../simulation/DOCKER_PORTAINER.md) are installed on both VMs

#### Remote Desktop

To interact with GUI tools, make sure remote desktop is installed and enabled on each VM:

```bash
sudo apt install xrdp
```

#### Configure Proxy on each VM

Ensure that the Network Proxy is configured in all the appropraite places on each VM, including:

- Docker
- Ubuntu's apt package manager
- Wget
- Git
- Firefox

#### Create a shared folder

So that all users can access the tools you need, create a shared folder in which applications will be installed in the following steps:

```bash
sudo mkdir /home/shared
sudo chmod 777 /home/shared
```

### Password Management

During the deployment process you will be required to define several secure passwords. It is recommended to make use of a password manager to store these passwords. You may consider using [KeePass](https://keepass.info/).

## KeyVM Installation

The following steps should be performed on the KeyVM.

### Configure Log File Directory

First make a directory in which to store log files:

```bash
sudo mkdir /data/logs
```

### Configure KeyDB Secrets

Next configure the following secrets in Portainer. Be sure to assign secure passwords to `keydb_pass` and `keydb_ctp_pass`. As you assign these, be sure to store these passwords in your password manager.

- keydb_user
- keydb_pass
- keydb_ctp_pass

### Deploy KeyVM Docker Stack

In portainer, create a new Docker Stack. Insert the contents of the KeyDB stack template found [here](https://github.com/AustralianCancerDataNetwork/auscat_installation/blob/main/docker-compose-key.yml).

Before deploying the stack, modify the PGAdmin login credentials by updating the environment variables for:

- PGADMIN_DEFAULT_EMAIL
- PGADMIN_DEFAULT_PASSWORD

Also update the log path folder voluem for CTP to:

```yaml
- /data/logs:/logs
```

You are now ready to deploy the stack.

### Test KeyVM Deployment

In a web browser, navigate to `http://[KEYVM-IP]:5050` and login to PGAdmin using the email address and password combination configured in the Docker stack.

In here, you will need to configure the connection to the KeyDB. Create a new connection and specify the following:

- Name: KeyDB
- Host: `[KEYVM-IP]`
- Port: 5433
- Username: `[keydb_user]` (configured as secret)
- password: `[keydb_pass]` (configured as secret)

You may also test out the web interface for CTP. Navigate to `http://[KEYVM-IP]:9090` and confirm you can access the CTP admin interface.

### Pentaho Installation

Instructions on installing and utilising Pentaho can be found [here](https://github.com/AustralianCancerDataNetwork/auscatverse/blob/pentaho-docs/components/PENTAHO.md). Ensure Pentaho is installed before proceeding to setting up the CatVM.

## CatVM

The following steps should be performed on the CatVM.

When deploying the CatVM, a large data disk should be available to host the imaging data. This may be the disk on which the operating system is installed, or it may need to be mounted, for example under `/mnt/data`.

### Configure CatDB Secrets

Next configure the following secrets in CatVM's Portainer. Be sure to assign a secure password to `catdb_pass`. Also be sure to store this in your password manager.

- catdb_user
- catdb_pass

You should also add a secret to hold the Orthanc configuration. Create a secret named `orthanc.json` with the contents [found here](https://github.com/AustralianCancerDataNetwork/auscat_imaging/blob/main/orthanc/orthanc.json) (making sure to set a secure username/password combination under registered users and storing these in your password manager):

### Deploy CatVM Docker Stack

In portainer, create a new Docker Stack. Insert the contents of the CatDB stack template found [here](https://github.com/AustralianCancerDataNetwork/auscat_installation/blob/main/docker-compose-cat.yml).

Before deploying the stack, modify the PGAdmin login credentials by updating the environment variables for:

- PGADMIN_DEFAULT_EMAIL
- PGADMIN_DEFAULT_PASSWORD

also update the Orthanc volume mapping to a location on the large data disk mounted (if this isn't the operating system disk):

```yaml
- /media/data/orthanc:/var/lib/orthanc/db
```

You are now ready to deploy the stack.

### Test CatVM Deployment

In a web browser, navigate to `http://[CATVM-IP]:5050` and login to PGAdmin using the email address and password combination configured in the Docker stack. Create a connection to the CatDB using the following credentials:

- Name: CatDB
- Host: `[CATVM-IP]`
- Port: 5434
- Username: `[catdb_user]` (configured as secret)
- Password: `[catdb_pass]` (configured as secret)

You may also want to log into the Orthanc admin area by visiting `http://[CATVM-IP]:8042` and entering the credentials you specified in the `orthanc.json` secret.

## Data Import

### Clinical Data

On the KeyVM, setup the clinical data import pipelines in Pentaho. First clone the repository containing the Pentaho templates:

```bash
cd /home/shared
git clone git@github.com:AustralianCancerDataNetwork/auscat_etl_centres.git
```

Run Pentaho, and them import the appropriate `.kjb` files and confgured the connection to the OIS (MOSAIQ/ARIA) for your centre. Consult the [Pentaho guide](https://github.com/AustralianCancerDataNetwork/auscatverse/blob/pentaho-docs/components/PENTAHO.md) for details on running Pentaho and configuring pipelines.

### Imaging Data

One initial clinical data has been imported, DICOM imaging data for those patients can be sent to the Orthanc running on the CatVM via the CTP for anonymisation. To achieve this, send the DICOM data to the following DICOM endpoint:

- Host: `[KEYVM-IP]`
- Port: 8104

## Troubleshooting

### A container is failing to deploy

Ensure that the image is being pulled properly. Sometimes a manual pull of an image can help resolve this. Navigate to `Images` in Portainer and pull the image which is failing to deploy from DockerHub. Once pulled, try to deploy the container in the stack again.

### Authentication error when logging into PGAdmin

PGAdmin may have issues with login if your password contains any special symbols (such as a $ sign). Change the password, remove the PGAdmin volume and redeploy the stack.
