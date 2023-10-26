# Authentication & Authorisation

> This document outlines the process for dealing with authentication and authorisation to conform to AusCAT ethics requirements. Future developments aim to streamline this process by implementing an authentication framework for use across all components. Read more [here](https://unsw.sharepoint.com/:w:/s/InghamPhysics504/EauyXapq2xlMpoeFOHhhwdoBeaNMw9021WykduL0LvC4Uw?e=WxUzqG).

## Roles

To fulfil AusCAT ethics requirements, the following roles have been defined:

- **Researcher**: Academic/clinical researchers conducting projects using AusCAT data.
- **Technical Staff**: Staff members approved to work at each node who assist with setup of AusCAT infrastructure.
- **Local hospital project lead**: The primary contact at each individual hospital/node participating in AusCAT.
- **Principal Investigator**: The Principal Investigator as stated in AusCAT ethics (currently Prof. Lois Holloway).

## Components

The following components in AusCAT are used which have grouped into ***identifiable*** and ***anonymised*** categories:

### ***Identifiable*** Components

The following AusCAT components host information which can be used to ***identify*** patients:

- [KeyDB](../components/KEYDB.md)
- [CTP Admin](../components/CTP.md)
- [PGAdmin](../components/PGADMIN.md) (with access to KeyDB)
- [KeyVM](PRODUCTION.md#vm-1-keyvm)
- [Portainer](../components/PORTAINER.md) (running on KeyVM)

### ***Anonymised*** Components

The following AusCAT components host ***anonymised*** patient data only:

- [CatDB](../components/CATDB.md)
- [PGAdmin](../components/PGADMIN.md) (with access to CatDB)
- [Orthanc](../components/ORTHANC.md)
- [Dashboard tools](../components/DASHBOARD.md)
- Ontology tools
- [CatVM](PRODUCTION.md#vm-2-catvm)
- [Portainer](../components/PORTAINER.md) (running on CatVM)
- Federated Learning Framework (e.g., Vantage6, AusCAT FL, etc...)

## Requirements

Given the roles and components stated above, the following should be observed to fulfil AusCAT ethics requirements:

- **Researchers** have access to anonymised data only at the centre they are working at (no access to ***identified*** data).
- **Researchers** are given access when added to the AusCAT project and access is revoked when they leave the AusCAT project.
- **Local hospital project lead** is responsible for granting/revoking **researcher** access.
- **Technical staff** may have temporary access to the ***identifiable***/***anonymised*** components for setup, however no access to ***identifiable*** and ***anonymised*** simultaneously after data has been imported.
- **Principal investigator** must approve **technical staff** access to ***identifiable*** components as required for a given period.

## Password Management

### Password Manager

A suitable password manger must be used at each node. It must be secured using a master password and encrypt the of the vault stored on disk. If unsure, you may consider using [KeePass](https://keepass.info/).

The vault should be stored in an appropriate location accessible to the **local hospital project lead**. The **local hospital project lead** is also responsible for storing the password manager master password.

### Password Generation

Passwords should be generated using a random password generator. If your password manager doesn't have this functionality built in, you may consider using [this web-based password generator](https://my.norton.com/extspa/passwordmanager?path=pwd-gen).

Passwords generated should be at least 12 characters long and be made up of a combination of lower and upper case characters as well as numbers. *Avoid using special symbols in AusCAT passwords as these aren't compatible with some components*.

### Process

Ensure the following process is followed during setup and management of AusCAT nodes:

- During setup (prior to any data import) passwords for each component will be generated and stored in the password manager by **technical staff**.
- Once setup is complete, control of the password vault will be handed to the **local hospital project lead** who will store the vault in an appropriate location and change the master password.
- The **Principal investigator** will instruct **local hospital project lead** to give a **researcher** or a **technical staff** member access to a set of passwords (either ***identifiable*** components or ***anonymised*** components but not both simultaneously). A time period for which access should be given will also be defined by the **principal investigator**.
- The **local hospital project lead** will retrieve the passwords from the password manager and share the passwords for the relevant components only to the individual.
- At the end of the time period for which access was granted the **local hospital project lead** will modify all passwords which were shared with that user and store the updated passwords in the password manager.
