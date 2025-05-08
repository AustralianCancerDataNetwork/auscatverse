# AusCAT Node Setup Summary

This page provides a **summary of the steps** for establishing an **AusCAT node** at a new hospital, clinical centre, or registry, after being approved as a site in the ethics protocol and has received local governance approvals. Detailed instructions for each step are available in separate pages, which can be accessed through the links provided below.

---

## 1. Infrastructure Setup

As a first step, visit the [Infrastructure Requirements](./INFRASTRUCTURE.md) page to establish the hardware infrastructure. This page provides details on the **hardware specifications** required for the server, including CPU, RAM, storage, and GPU recommendations for running AusCAT software tools, extracting relevant tabular and image data, executing code, and storing results.

---

## 2. Production Deployment

Once the hardware and server operating system (e.g., **Windows Server 2022**) are ready, navigate to the [Production Deployment Guide](./PRODUCTION.md) for detailed steps on deploying an AusCAT node in a production setting. This includes the following components:

### 2a. VM Setup
- Install **two VMs** on the server:
  - **KeyDB VM**: For hosting the identifiable patient database
  - **CatDB VM**: For hosting the anonymised research database
- Detailed VM setup instructions can be found [here](./VM-SETUP.md).

### 2b. Docker & Portainer Installation
- Install **Docker** and **Portainer** on each VM running Ubuntu. This is required to manage containerized applications and services.
- Refer to the [Docker and Portainer Installation Guide](../simulation/DOCKER_PORTAINER.md) for step-by-step instructions.

### 2c. Configure and Deploy KeyVM Docker Stack
- Once Docker and Portainer are installed, configure **KeyDB secrets** and deploy the [KeyDB](../components/KEYDB.md) Docker stack on the **KeyDB VM**.
- Use the **KeyDB stack template** provided [here](https://github.com/AustralianCancerDataNetwork/auscat_installation/blob/main/docker-compose-key.yml).
- Test the deployment to confirm the stack is running correctly.

### 2d. Pentaho Installation
- Install **Pentaho**, a data integration tool, on the **KeyDB VM**.
- Detailed installation steps can be found [here](../components/PENTAHO.md).

### 2e. Configure and Deploy CatVM Docker Stack
- On the **CatDB VM**, configure **CatDB secrets** and deploy the [CatDB](../components/CATDB.md) Docker stack.
- Use the **CatDB stack template** provided [here](https://github.com/AustralianCancerDataNetwork/auscat_installation/blob/main/docker-compose-cat.yml).
- Test the deployment to confirm the stack is running correctly.

---

## 3. Data Import

Once the infrastructure is in place, navigate to the data import pages to begin importing clinical and imaging data from the hospital's clinical systems into the AusCAT node.

### Connection Parameters
Ensure that proper authentication is configured for **Pentaho** to connect to the clinical systems. Without this, data extraction will not be possible. Typically, data is extracted from the **reporting server** rather than the live server for security reasons.

| **Usage** | **Source IP** | **Source Port** | **Destination IP** | **Destination Port** | **Protocol** | **2-Way** |
|------------|---------------|-----------------|--------------------|---------------------|--------------|-----------|
| SQL Queries | xxx.xxx.xxx.xxx (Reporting Server) | 1433 | xxx.xxx.xxx.xxx (KeyDB) | 104 | TCP | Yes |

### 3a. Clinical Data
- Once the Pentaho connection is established, navigate to the [Pentaho Configuration Guide](../components/PENTAHO.md) for details on running pipelines to populate the research database with clinical data items.

### 3b. Imaging Data
- After initial clinical data is imported, DICOM imaging data for those patients can be sent to the **Orthanc** server for storage and later processing.

---

## Node Setup Checklist

- **Ethics and Governance Approval:** Site is listed on current approved ethics protocol and local governance approvals are in place.
- **Server Hardware**: The centre has provisioned server hardware meeting the minimum or optimal specifications.
- **Operating System**: The host server is running **Windows Server 2022**.
- **VM and Docker Installation**: Two VMs have been created and Docker/Portainer installed as per the infrastructure guide.
- **Deploy Docker Stacks**: Respective docker stacks have been deployed on both VMs.
- **Authentication and Permissions**: Appropriate credentials and access permissions for connecting the clinical systems from data integration tool (e.g. Pentaho) have been obtained.
- **Clinical Data Import**: Pentaho is installed and run for clinical data setup.
- **DICOM Data Import**: Orthanc server is set up for exporting and storing DICOM imaging data from the clinical systems.

---

For additional support or questions, please contact the AusCAT technical team.
---
- Fahim Alam
- Sasha Barisic
- Amir Anees