# Day 1

## Hypervisor Overview
<pre>
- is hardware virtualization technology
- this helps us run multiple Operating System on a laptop/desktop/workstation/server
- i.e more than one OS can be actively running on the same machine
- this type of virtualization is considered heavy-weight as each Virtual Machine(VM - Guest OS) requires dedicated hardware resources
  - CPU Cores
  - RAM
  - Storage ( HDD/SDD )
- each VM represents a fully functional Operating System
- the OS that runs within Virtual Machine is referred as Guest OS
- each Guest OS has its own dedicated OS Kernel
- there are two of Hypervisors  
  1. Type 1
  - Bare-Metal Hypervisors
  - Used in Servers & Workstations
  - Virtual Machines(Guest OS) can be created directly on top of hardware with no Host OS requirement
  - Examples
    - VMWare vSphere/vCenter
  2. Type 2
  - used in Laptops/Desktops/Workstations
  - Examples
    - Oracle VirtualBox ( supported in Windows/Linux/Mac OS-X )
    - Parallels ( supported in Mac OS-X )
    - KVM ( supported in all Linux Distributions )
    - VMWare Fusion ( supported in Mac OS-X - Free, earlier it was a paid software, not actively maintained anymore )
    - VMWare Workstation Pro ( was a paid software but now it is Free, works in Linux & Windows )
    - Microsoft Hyper-V
</pre>

## Container Overview
<pre>
- is an application virtualization technology
- each container represents one application
- containers can't run a OS inside it, they can only run a single application
- hence, containers are not a replacement for Hypervisor(Virtualization)
- in practical world, Virtualization and Containerization are used in combination, hence they are complimenting technology and not a competing technology
- is considered light-weight virtualization
- containers running in the Host OS, shares the hardware resources on the underlying Host OS
- containers don't get their own hardware resources
- contianers does'nt have OS Kernel
- containers depend on the Host OS Kernel for any OS functionality
</pre>

## Docker Overview

## High-Level Docker Architecture
![Docker](DockerHighLevelArchitecture.png)

## Demo - Installing Docker Community edition in RHEL or Oracle Linux
```
sudo yum install -y yum-utils
sudo yum-config-manager --add-repo https://download.docker.com/linux/rhel/docker-ce.repo
sudo yum install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin
sudo usermod -aG docker $USER
sudo systemctl enable docker
sudo systemctl start docker
sudo systemctl status docker
docker --version
docker info
docker images
```

Expected output
![image](https://github.com/user-attachments/assets/d44d038b-18e7-4228-8aec-d2707589500c)
![image](https://github.com/user-attachments/assets/0575593b-4168-4657-ab6d-1437dfaba83a)
![image](https://github.com/user-attachments/assets/3cc2e47a-52be-4512-bfde-289c16c629c6)
![image](https://github.com/user-attachments/assets/f84d3e86-35ae-4a60-8cb5-4188fbdcf474)
![image](https://github.com/user-attachments/assets/a733e6a1-37ce-4fe5-a7a5-32fcf4454ea4)


## Demo - Download a docker image from Docker Remote Registry to Local Docker Registry
```
docker images
docker pull nginx:latest
docker images
```

Expected output
![image](https://github.com/user-attachments/assets/4bac0c5b-5b43-4d8b-82c1-a59b2248a904)

## Demo - Creating an nginx container
```
docker run -d --name nginx1 --hostname nginx1 nginx:latest
docker ps
docker ps -a
```

Expected output
![image](https://github.com/user-attachments/assets/bc33264a-0bdd-41c8-8506-7a507dc647ae)


## Container Image Overview
<pre>
- Container image is template or specification or blueprint of a container
- It is similar to Windows-12-OS-DVD-image.iso 
- Just like how we are able to use windows DVD iso image and install Windows 12 OS on multiple machine, on the similar fashion, using a Container Image one can create multiple containers
- Containers are served by Container Registries
- Examples
  - Ubuntu Container Image ( ubuntu:24.04 )
  - Nginx Container Image ( nginx:latest )
  - 
</pre>  

## Container Registries Overview
<pre>
- Docker supports 3 types of Container Registries
  1. Local Docker Registry
  2. Private Docker Registry and
  3. Remote Docker Registry ( Docker Hub Website -  hub.docker.com )
- Local Docker Registry is just a folder, where all the docker images are stored
- Private Docker Registry is server that can be setup using JFrog Artifactory or Sonatype Nexus
- Remote Docker Registry is a website that has a whole lot of Docker Images
</pre>  

## Containers Overview
<pre>
- Container is an instance of one Container Image
- Whatever software tools are present in the Container Image are readily available for use in any container intance
- containers gets its own Private IP address
- containers get its own file system
- containers supports one or more shells
- containers has its own network stack ( 7 OSI Layers )
- containers has its own software defined network cards ( Network Interface Card - NICs )
- each container represents one instance of an application
- containers are not OS
- some containers may appear like a OS, but technically they are just application process that runs in a separate namespace
</pre>  

## Container Orchestration Overview
<pre>
  
</pre>

## Red Hat Openshift Overview
<pre>
  
</pre>

## High-Level Openshift Architecture
<pre>
  
</pre>
