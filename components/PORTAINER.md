# Portainer Community Edition  

## Home Page  

In the left-hand side, click the `Home` button, you will see an overview of your environments along with vital statistics about each.  

For example, In AusCAT, you will see a Docker icon as the primary environment, it shows the number of Docker stacks, services, containers, volumes etc., on the host machine.  

Your currently selected environment will be shown by the connected Status on the right. 

## Docker Swarm 

### Dashboard  

The Docker/Swarm dashboard summarizes the Docker Swarm environment and shows the components that make up the environment. 

#### Cluster information  

This section shows how many nodes are in the clusters and a link to the cluster visualizer (a visual representation of the nodes in your cluster and the tasks on each node). 

#### Summary tiles 

The remaining dashboard is made up of tiles showing the number of stacks, services (for Docker Swarm), containers (including health and running-status metrics), images (and how much disk space they consume), volumes and networks, and GPUs (if enabled). 

### App Templates 

An app template lets you deploy a container (or a stack of containers) to an environment with a set of predetermined configuration values while still allowing you to customize the configuration (for example, environment variables). 

### Stacks  

A stack is a collection of services, for example, in AusCAT, one stack contains multiple Docker containers.  

#### Add a new Stack 

Deploy a new stack from Portainer by using Web editor. From the menu select `Stacks`, click `Add stack`, give the stack a descriptive name then selects `Web editor`. We usually copy-paste the Docker stack to the web editor to define the services.  

#### Inspect a Stack 

From the menu select `Stacks` then select the stack you want to inspect. From here you can stop, delete or create a template from the stack.  

Scroll down the webpage, you will see a Services list. For example, we use Docker Swarm in the AusCAT, you can: 

* View the services that make up the stack. 
* Check to see if they are running or stopped. 
* See how many replicas are running on each host. 
* Get access to logs. 
* Inspect individual services. 
* View service statistics. 
* Get access to the service's console. 

#### Edit a Stack  

In AusCAT, the stack was deployed using the Web Editor, you will have the option to edit your compose file manually. 

### Services 

This menu is only available for Docker Swarm endpoints, a service consists of an image definition and container configuration as well as instructions on how those containers will be deployed across a Swarm cluster. 

For example, you will see Stack name, Image name, Scheduling Mode, Published Ports, Last Update and Ownership.  

Click the arrow next to service name, you will see more useful details of its status, under the `Actions` column, the first icon shows the `logs` of the service, the second icon enables you to `inspect` the service, the third icon shows some `statistics` of the service, the fourth icon enables you to interact with the container by using commands in a `console`.  

### Containers 

This page shows a list of containers on the host. Containers do not hold any persistent data and therefore can be destroyed and recreated as needed. 

Under the `Quick Actions` column, the first icon shows the logs of a container, the second icon enables you to inspect a container.  

The Stack column shows the stack that the current contains sits in. 

The Image shows the Image this container is using. A container is a runnable instance of an image. 

### Images 

#### Pull image  

You can easily pull the image from a private Dockerhub account through this page if you correctly put the credentials in the Swarm Registries section (at the end of this document). 

You can type in the docker image name and click `Pull the image`. 

### Networks 

### Volumes 

### Configs  

### Secrets 

### Events 

### Host 

### Swarm  

#### Setup 

#### Registries 

##### Add AusCAT Docker Registry  

You will need add the DockerHub registry token to Portainer so that you can fetch the required AusCAT images. Navigate to Registries in the left menu and click Add Registry. Enter the credentials and token you obtained from the AusCAT team. 

 

 