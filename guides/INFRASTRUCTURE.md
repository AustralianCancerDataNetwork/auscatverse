# AusCAT Infrastructure Requirements

This document outlines a typical AusCAT setup. These requirements are flexible and the system can be run outside of these recommendations. For more information please discuss with your centre's AusCAT contact.

## Server Hardware

### Minimal Specs 
The minimum specifications we recommend for a centre's server hardware to be installed and run in-house are:

- 8 core CPU
- 16 GB RAM
- 1x 512GB SSD Disk (Usually faster, to run the operating system and software)
- 1x 1TB SSD Disk (Might be slower and will store data)

This hardware will typically be installed and managed by your IT department. They will usually install Microsoft Windows Server 2019 where you can then create one or more Virtual Machines to run AusCAT.

### Optimal Specs
A typical centre's server hardware to be installed and run in-house has approximately the following specification:

- 24 core CPU (e.g. 2x Intel Xeon Silver 4214R @2.4Ghz 12 Cores)
- 128-256GB RAM
- 2x 512GB SSD Disk (Usually faster, to run the operating system and software)
- 4x 4TB SSD Disk (Might be slower and will store data)
- 1x NVIDIA Quadro RTX 6000-8000 (Optional GPU, required for deep-learning/auto-segmentation projects)

Typical cost (note this can vary significantly over time): $20,000-$30,000

This hardware will typically be installed and managed by your IT department. They will usually install Microsoft Windows Server 2019 where you can then create one or more Virtual Machines to run AusCAT.

## Virtual Machine

A Virtual Machine with the following specifications is typically recommended:

- 24 virtual CPU Cores
- 128GB RAM
- 1x fixed size 256GB virtual disk (stored on the faster SSD)
- 1x dynamically expanding virtual disk (stored on the slower SSD)
- Operating System: Install Ubuntu Desktop 22.04 on the Virtual Machine (Desktop edition is chosen to provide a GUI for certain applications)

As a minimum, the following is suggested:

- 8 virtual CPU Cores
- 16GB RAM
- 1x fixed size 128GB virtual disk (stored on the faster SSD)
- 1x dynamically expanding virtual disk (stored on the slower SSD)
- Operating System: Install Ubuntu Desktop 22.04 on the Virtual Machine (Desktop edition is chosen to provide a GUI for certain applications)

> GPU Passthrough: Follow [these instructions to pass through the GPU](https://docs.microsoft.com/en-us/windows-server/virtualization/hyper-v/deploy/deploying-graphics-devices-using-dda?source=recommendations) to this VM if you have one.

## Cloud Solution

Many organisations are now enabling use of cloud resources (such as Azure or AWS) within their internal network.

If you are unsure about cloud solutions, it's best to discuss this with your IT department.

Virtual Machine setup in the cloud is somewhat more complicated than when hosting the infrastructure in-house. This is because certain cloud resources can be quite costly, so multiple VMs with differing resources can be setup on only turned on when needed.

## Network Proxy

To streamline installation and deployment of the AusCAT tools, your will require access to certain locations via your organisations proxy.

Otherwise you will need to request the following locations to be whitelisted for your server within your organisation:

- Ubuntu
  - archive.ubuntu.com
  - au.archive.ubuntu.com
  - security.ubuntu.com
  - archive.canonical.com
  - repo.mongodb.org
  - keyserver.ubuntu.com
  - changelogs.ubuntu.com
- Docker
  - *.docker.io
  - *.docker.com
- GitHub
  - *.github.com
  - *.github.io
  - *.ghcr.io

## VM Proxy Configuration

There are multiple spots you should define your proxy within the Ubuntu Virtual Machine:

### Apt Config

Create a file named: `/etc/apt/apt.conf.d/proxy.conf` (You will need to use `sudo` to write to this location).

Add the following information into this file, replacing the configuration with your proxy config:

```text
Acquire::http::Proxy "http://proxy_host:proxy_port/";
Acquire::https::Proxy "https://proxy_host:proxy_port/";
```

Then run:

```bash
sudo apt update
```

and ensure that there are no errors.

### Git (GitHub)

#### Access via SSH

Add the following to your SSH config (located at ~/.ssh/config):

```text
Host github.com 
 User git 
 ProxyCommand nc -x proxy_host:proxy_port -Xconnect %h %p
```

And be sure to [setup SSH keys between your GitHub account and your VM](https://docs.github.com/en/authentication/connecting-to-github-with-ssh).

#### Access via HTTP

If you use Git via HTTP, you can configure the proxy via the command line:

```bash
git config --global http.proxy http://proxy_host:proxy_port
```

You may also have to skip SSLVerify (depending on your proxy setup, if this is needed access via SSH is preferred)

```bash
git config --global http.sslVerify false
```

## Docker

We use [Docker](https://www.docker.com/) to deploy ready to go containers which run the AusCAT software. Follow [these instructions to install Docker](https://docs.docker.com/engine/install/ubuntu/) within your Ubuntu virtual machine. We also have a guide for installing Docker in a simulation (not suitable for production)  environment which can be found [here](../simulation/DOCKER_PORTAINER.md#install-docker).

### Docker installation in secure environments

In some secure and locked-down environments, there may be more than one proxy available. One proxy may not required authentication, but it will be more restrictive in what traffic can get through. This is the proxy where ideally the URLs listed above would be whitelisted.

You can setup the proxy on your VM for your local account by editing the ```~/.bash_profile``` file and adding these these lines:

```bash
export http_proxy=http://proxy_host:proxy_port
export https_proxy=http://proxy_host:proxy_port
export no_proxy=localhost, 127.0.0.1
```

In scenarios where this does not work, you may need to add the other proxy, with authentication to your config. In the example of installing Docker, you would modify `/etc/apt/apt.conf.d/proxy.conf` to:

```text
Acquire::http::Proxy "http://username:password@proxy_host:proxy_port/";
Acquire::https::Proxy "https://username:password@proxy_host:proxy_port/";
```


> *NOTE: If an error of the form "```SSL routines::unexpected eof while reading```" appears when running Docker commands, you can fix this by updating the SSL package  on your VM by running:*
> ```
> sudo apt update && sudo apt upgrade -y
> ```

Once Docker has been installed, to access Dockerhub images, Docker proxy access must be configured. Add the following lines to the file ```/etc/systemd/system/docker.service.d/http-proxy.conf```:
```
[Service]
Environment="HTTP_PROXY=http://proxy_host:proxy_port"
Environment="HTTPS_PROXY=http://proxy_host:proxy_port"
Environment="NO_PROXY=localhost,127.0.0.1"
```

Then restart the docker service:
```bash
sudo systemctl daemon-reload
sudo systemctl restart docker
```

