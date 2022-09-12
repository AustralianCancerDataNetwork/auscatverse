# AusCAT Infrastructure Requirements

This document outlines a typical AusCAT setup. These requirements are flexible and the system can be run outside of these recommendations. For more information please discuss with your centre's AusCAT contact.

## Server Hardware

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
- 32GB RAM
- 1x fixed size 128GB virtual disk (stored on the faster SSD)
- 1x dynamically expanding virtual disk (stored on the slower SSD)
- Operating System: Install Ubuntu Desktop 22.04 on the Virtual Machine (Desktop edition is chosen to provide a GUI for certain applications)

> GPU Passthrough: Follow [these instructions to pass through the GPU](https://docs.microsoft.com/en-us/windows-server/virtualization/hyper-v/deploy/deploying-graphics-devices-using-dda?source=recommendations) to this VM if you have one.

## Cloud Solution

Many organisations are now enabling use of cloud resources (such as Azure or AWS) within their internal network.

For NSWHealth organisations this is already possible and has been partially tested. If you'd like to learn about the process of obtaining a self-managed cloud instance you can [follow the instructions on SARA](https://sara.health.nsw.gov.au/customerportal?id=kb_article_view&sysparm_article=KB0010171&sys_kb_id=0e3631291b48781008fdc95c274bcb44&spa=1).

For organisation outside of NSWHealth it's best to discuss cloud options with your IT department.

Virtual Machine setup in the cloud is somewhat more complicated than when hosting the infrastructure in-house. This is because certain cloud resources can be quite costly, so multiple VMs with differing resources can be setup on only turned on when needed.

## Network Proxy

To streamline installation and deployment of the AusCAT tools, your will require access to certain locations via your organisations proxy.

If your centre is governed by NSWHealth, this has already been enabled, contact your AusCAT centre's representative for proxy details.

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

We use [Docker](https://www.docker.com/) to deploy ready to go containers which run the AusCAT software. Follow [these instructions to install Docker](https://docs.docker.com/engine/install/ubuntu/) within your Ubuntu virtual machine.

### Docker installation in NSWHealth

In some environments, such as NSW Health, there may be more than one proxy available. One proxy may not required authentication, but it will be more restrictive in what traffic can get through. This is the proxy where ideally the URLs listed above would be whitelisted.

In scenarios where this does not work, you may need to add the other proxy, with authentication to your config. In the example of installing Docker, you would modify `/etc/apt/apt.conf.d/proxy.conf` to:

```text
Acquire::http::Proxy "http://username:password@proxy_host:proxy_port/";
Acquire::https::Proxy "https://username:password@proxy_host:proxy_port/";
```
