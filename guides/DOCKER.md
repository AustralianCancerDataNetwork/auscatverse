# Docker and AusCAT

This document aims to give a description of the Docker tool and to describe how and why it is utilised as the key part of the AusCAT framework.

> Within hospital/corporate IT environments you may be required to obtain approval to run a tool like Docker. This document should provide the information needed for this approval. If there is additional information required, this would ideally be added to this document.

## Docker Description

Docker is a tool to enable packaging and deployment of software applications and environments. It uses the concept of containerisation which bundles tools, libraries and configuration files in an isolated environment. These bundles are built as Docker Images, and when deployed are referred to as Docker containers. This ensures that when the same containers are deployed on different servers, the same environment and therefore functionality is ensured.

Systems deployed using Docker typically consist of many containers, each responsible for one component of the system. Communication between containers is enabled via well-defined channels.

The concept of Docker containers is often compared to virtual machines (VMs) since they also provide an isolated operating environment. The key difference between the two is that Docker containers are much more lightweight than VMs as they share the services of the operating system kernel, unlike VMs which each run their own.

## Justification for use of Docker in AusCAT

The AusCAT infrastructure consists of many different components, including databases, DICOM PACS, ETL tools and federated learning tools. Each of these components rely on various libraries and configuration. The manual installation of these is time consuming and challenging due to different operating system environments across centers. Updating individual components is also difficult to orchestrate and fragmentation of software versions across AusCAT centres is likely.

By encapsulating all of these tools within Docker containers many of these logistical issues are resolved. The AusCAT development team can ensure that all Docker containers are tested and functioning in a simulation environment before the exact same containers is deployed across centres. It won't matter what operating system the host machines are running at each centre provided they can run Docker. Like this we can ensure consistent functionality across the AusCAT centres.

## Docker Installation

To harness the full benefits of Docker, ideally it should be installed on a Linux Operating System. The enables containers to optimally share the OS services with the Linux kernel. In cases where Linux can't be run directly, Docker can be installed using the Windows Subsystem for Linux.

Docker installation instructions can be found [here](https://docs.docker.com/engine/install/). Note that the Docker Desktop edition may be useful for development purposes. However please ensure that your organisation meets the licence criteria for Docker Desktop. If not, then installing the Docker engine directly is the best option.

You may also like to consult the [AusCAT infrastructure guide](https://github.com/AustralianCancerDataNetwork/auscatverse/blob/main/guides/INFRASTRUCTURE.md) and the [AusCAT installation step-by-step guide](https://github.com/AustralianCancerDataNetwork/auscat_installation/blob/main/README.md) for more information on how Docker is intended be installed and utilised in the AusCAT project.

## Security Considerations

While using Docker might help alleviate some security concerns, it will also likely pose additional concerns for IT departments at which it is deployed. Here some considerations are outlined with discussion points to have with IT security managers. Where issues are identified, these would ideally be fed back to the AusCAT development team so that these can be addressed across all centres.

### Snyk

All Docker images (which are used to deploy containers) used by AusCAT are hosted on [DockerHub](https://hub.docker.com/). Each version of these images is scanned by [Snyk](https://www.snyk.io). This will alert us of any known security vulnerabilities found so that these can be rectified before deploying to an AusCAT centre.

### Inherited images

Since AusCAT Docker images will build on top of an existing image, it is essential that these are safe and secure to use. AusCAT images will only inherit from well established open-source images (such as Ubuntu, Postgres). If a security vulnerability was to present itself at that level, the community generally finds these quickly and Snyk will make us aware of the issue so that it can be rectified.

### Container Deployment

We utilise a tool named [Portainer](https://www.portainer.io/) (which is also deployed as a Docker image) to manage and deployment of AusCAT Docker containers at centres. This provides an easy to use interface which can be managed by a technical representative at the centre. Like this they can properly inspect the Docker images which are to be deployed (and the source code which built them on our GitHub) before clicking the button in Portainer to pull the image from DockerHub and deploy it at the centre.

### Containers running as root

By default, Docker containers run as the root user of that system. Thanks to the isolation of the containers themselves this shouldn't pose a problem. However it is essential that access to the system running Docker is permitted only for authorised users.

### Network Proxy Whitelisting

For streamlined deployment, a network proxy should be configured to allow traffic from DockerHub. This allows centre's to easily fetch new and updated AusCAT images to deploy at their centre. The following two sub-domain ranges should be whitelisted:

- *.docker.io
- *.docker.com

## Resources

Here are some useful resources to find out more about Docker. Please don't hesitate to contact your AusCAT technical representative for more information.

- [Docker Installation](https://docs.docker.com/engine/install/)
- [Docker Security](https://docs.docker.com/engine/security/)
- [DockerHub](https://hub.docker.com/)
- [Portainer](https://www.portainer.io/)
- [Running Docker on Windows Subsystem for Linux](https://docs.docker.com/desktop/windows/wsl/)
