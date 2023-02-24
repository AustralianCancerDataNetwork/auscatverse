# AusCAT Simulation Environment

To enable learning, development and testing of the AusCAT system a simulation environment can be deployed which loads some public data obtained from The Cancer Imaging Archive. Within this environment the entire system can be run end-to-end to help prepare for deployment of a production ready system at participating centres/sites.

> CAUTION: The simulation environment deals only with public data. Therefore some default passwords are used to enable quick and easy deployment of the simulation setup. These are not suitable for use in a production system and care should be taken to set secure passwords on a production environment.

## Virtual Machine Setup

### NECTAR Cloud

> You will require a University affiliation (with login) to use the NECTAR cloud.

Perform the following steps to setup a virtual machine on the NECTAR cloud in which to deploy a simulation environment.

1. [Login to NECTAR](https://dashboard.rc.nectar.org.au/project/) using your University affiliation credentials.

2. Switch to the `ACDN-AusCAT-Sim` project. If you are not already a member of this project contact you AusCAT representative for access.

    <img src="images/NECTAR_1.png"  height="200px">

3. Navigate to `Compute->Instances` and press `Launch Instance`

4. Choose an Instance Name, preferably including your name so this can be identified as your VM (e.g. AusCat-Sim-Phil). Press Next.

    <img src="images/NECTAR_2.png"  height="300px">

5. Select a Source image for the VM. NECTAR supplies many images from which to create VMs, ideally you will select one based on Ubuntu. The ideal base image, without anything else installed is: `NeCTAR Ubuntu 20.04 LTS (Focal) amd64`. Search for it, press the up arrow to make sure it appears under allocated.

    <img src="images/NECTAR_3.png"  height="300px">

6. Next select a flavour. This determines the amount resources that will allocated to the VM. Choose one with a low SU/hour as this is the amount of credits that will be used within our project. If you are experimenting and don't need your VM to stay longer than 24 hours, best to select a preemptible flavour (note the yellow alert symbol) as it will be deleted automatically. Press the up arrow to allocate.

    <img src="images/NECTAR_4.png"  height="300px">

7. Use the default Network settings, press next.

8. Allocate the appropraite predefined security groups (press the up arrow):

    <img src="images/NECTAR_5.png"  height="300px">

9. Generate an SSH keypair and add it to your VM instance. A detailed description on how this is done can be found here: [https://tutorials.rc.nectar.org.au/keypairs/03-ssh-keygen](https://tutorials.rc.nectar.org.au/keypairs/03-ssh-keygen).

10. You're now ready to Launch your Instance!

Wait a few minutes for you VM to be ready. Once built, you can SSH into your VM by finding the IP address assigned:

    <img src="images/NECTAR_6.png"  height="300px">

Then sign in with:

    ```bash
    ssh -i ~/.ssh/your-private-ssh-key.pub ubuntu@your-vm-ip
    ```

You should now be logged in to your NECTAR VM.

## Docker + Portainer Installation

### Install Docker

### Install Portainer

### Add AusCAT Docker Registry

## AusCAT Simulation Environment Deployment

### Deploy the simulation Docker stack

### Import simulation data
