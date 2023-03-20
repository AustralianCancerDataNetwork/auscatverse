# Docker & Portainer

This document describes how to install Docker and Portainer on a host machine running Ubuntu.

## Install Docker

The Docker documentation describes in detail how to install Docker on Ubuntu. Follow the steps here: [https://docs.docker.com/engine/install/ubuntu/](https://docs.docker.com/engine/install/ubuntu/). Don't forget to also run the Linux post-install steps: [https://docs.docker.com/engine/install/linux-postinstall/](https://docs.docker.com/engine/install/linux-postinstall/).

Once Docker is successfully installed, initialise a swarm which will be used in the next step:

```bash
docker swarm init
```

**If you're using a NECTAR VM with multiple IP addresses assigned, use the following command instead and replace `VM_IP_ADDRESS` with the main IP address you use to connect:**

```bash
docker swarm init --advertise-addr VM_IP_ADDRESS
```

## Install Portainer

Portainer provides a web-interface to manage your Docker instance. It isn't essential but it eases the depolyment process. Ensure you deploy Portainer within Docker swarm to enable additional features for service deployment. Detailed instructions can be found here: [https://docs.portainer.io/start/install-ce/server/swarm/linux](https://docs.portainer.io/start/install-ce/server/swarm/linux).

Once installed (and running, confirm with `docker ps` command), navigate to the Portainer dashboard in your web browser:

`http://your-vm-ip:9000`

You will be prompted to create a user account the first time you visit this dashboard.

## Add AusCAT Docker Registry

Next you will need add the Dockerhub registry token to Portainer so that you can fetch the required AusCAT images. Navigate to `Registries` in the left menu and click `Add Registry`. Enter the credentials and token you obtained from the AusCAT team:

![Add Registry](images/Portainer_1.png)

Test out pulling an image by navigating to `Home`. Then select the `Primary` environment and choose `Images` from the left menu. Try to pull the `auscat/default_ctp:latest` (or any other AusCAT image) from the registry you added. Confirm that the image was pulled successfully.

![Pull Image](images/Portainer_2.png)
