# A template containing sub-sections for Docker component documentation should be prepared

## Overview
* Brief introduction to the docker component

* Explanation of the purpose and functionality of the component 

* Usage examples (Provide examples of how to use the image, including command-line examples and any relevant configuration options)

* Dependencies (List any dependencies required by the image, such as other Docker images)

* Ports and network access (Specify any ports that the image exposes and any network access requirements)

## Configuration
* Outline any configuration options that can be set when running the image (e.g., environment variables, command-line arguments, or configuration files), including a description of what they do and what types of values they accept

* Recommended usage patterns (Give guidance on how the image should be used in different environments, such as development, testing, or production)

## Access, Authentication, Secrets
* Access controls (Describe how access to Docker containers and images is controlled, including the use of user accounts, groups, roles, or permissions)

* Registry authentication (Outline how AusCAT Docker registry authentication is handled)

* User authentication (Describe how Docker handles user authentication and authorization, including how users are authenticated and authorized to access Docker containers and images)

* Secrets (Explain how to pass sensitive data to Docker Swarm on Portainer)

## Docker Image
* Name (Provide the name of the image and its version)

* Tag (if applicable)

* Upgrading (Explain how to upgrade the Docker image on Portainer, including best practices for managing image tags and version control)

* Description of base image used (and link to it on DockerHub

 * Path to Dockerfile and instructions for building (if needed)

## Docker Image extension (centre-varying images/project specific images)
* Outline the potentially centre-varying images where different images/tags are appropriate (eg. the MOSAIQ vs ARIA DB variations) or the project specific images (eg. the auscat_cardiac images),or even create new images that will integrate nicely with our stack.

* Contribution framework/pipeline (expand on below):
    * Build and test image in simulation environment
    * Create a PR to appropriate repo where the source code will sit and push the image to GHCR
    * Once AusCAT team test image from GHCR and approve PR, push final image to the AusCAT Dockerhub 

## Troubleshooting
* Common issues (List some of the most common issues that users may encounter when working with the Docker image. This can include errors, warnings, or unexpected behaviors that users might encounter, along with explanations of what the errors mean and how to fix them.)

* Diagnosing problems (Explain how to diagnose problems with the Docker image. This can include tools such as Docker logs on Portainer, Docker inspect)

* Support resources (Provide information on where users can go for support if they encounter problems with the Docker image. This can include links to documentation, forums, AusCAT technical contact, or support channels)

