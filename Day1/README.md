# Day 1

## About our lab environment
- OnPrem Production grade Red Hat OpenShift setup 
- System Configuration
  - 48 virtual cores
  - 755 GB RAM
  - 17 TB HDD Storage
- Oracle Linux v9.4 64-bit OS
- KVM Hypervisor
- 7 Virtual machines
  - Master 1 with RHEL Core OS ( 8 Cores, 128GB RAM, 500 GB HDD )
  - Master 2 with RHEL Core OS ( 8 Cores, 128GB RAM, 500 GB HDD )
  - Master 3 with RHEL Core OS ( 8 Cores, 128GB RAM, 500 GB HDD )
  - Worker 1 with RHEL Core OS ( 8 Cores, 128GB RAM, 500 GB HDD )
  - Worker 2 with RHEL Core OS ( 8 Cores, 128GB RAM, 500 GB HDD )
  - HAProxy Load Balancer, Bind DNS - Bastion Virtual Machine
  - BootStrap Virtual Machine
    - installs control plane components in master nodes
    - creates an etcd cluster configured etcd pod instances from respective master nodes
    - configures the HAProxy Load Balancer to act as a load-balancer the master nodes
    - helps in installing worker node components and join to the worker nodes to the openshift cluster

## What is dual-booting or multi-booting?
- Let's say we have a laptop with Windows 10 pre-installed and we need to do some prototype in Ubuntu
- You can use a system utility called Boot Loader like LILO, Grub 1, Grub2
- When we install boot loaders, it gets installed in your hard disk Master Boot Record(MBR) - Sector 0, Byte 0 in your hard disk (512 bytes)
- When we boot our machine, BIOS POST (Power On Self Test), once the BIOS is loaded, BIOS will initialize all hardwares and then it then it instructs the CPU to load and run the Boot loader application from MBR
- The boot application starts scanning for Operating Systems installed on your Hard disk(s), if it finds more than one OS then it gives a menu for you to choose which OS you wish to boot into
- Only one OS can be active at any point of time
Examples
- LILO
- GRUB
- BootCamp

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

## Linux Kernel Container Features
1. Namespace
   - helps in isolating one container from other containers running on the same OS
      
2. Control Group(CGroups)
   - to ensure every container shares the hardware resources co-operatively Control Groups are used
   - if this isn't not done, at times certain containers uses all the hardwares resources leaving other containers to starve
   - helps in applying resource quota restrictions like
     - we can restrict a container on how many CPU Cores it can use at the max at any point of time
     - we can restrict how much RAM a container use at the max
     - we can restrict, how much storage a container can use at the max

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
<pre>
- Docker is a Container Engine
- Developed in Golang by a company called Docker Inc
- comes in 2 flavours
  1. Docker Community Edition - Docker CE ( Free )
  2. Docker Enterprise Edition - Docker EE ( Paid )
- follows Client/Server Architecture
- Docker Registry
  - collection of many Docker Images
- Supports 3 types of Docker Registries
  1. Local Docker Registry
  2. Private Docker Registry
    - setup using JFrog Artifactory or Sonatype Nexus
  3. Remote Docker Registry
    - website maintained by Docker Inc 
    - provides many opensource docker images  
</pre>

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


## Lab - Download a docker image from Docker Remote Registry to Local Docker Registry
```
docker images
docker pull nginx:latest
docker images
docker pull redis:7.4
docker images
```

Expected output
![image](https://github.com/user-attachments/assets/4bac0c5b-5b43-4d8b-82c1-a59b2248a904)
![image](https://github.com/user-attachments/assets/2c69bfba-fb50-4a7b-a592-de53f1ae8d86)

## Lab - Creating an nginx container
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
</pre>  

## Container Registry Overview
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

## Info - Container Runtime
<pre>
- is a low-level software that is used to manage Containers and Images
- it is not so user-friendly, hence end-users generally don't use this directly
- examples
  - runC 
  - CRI-O
</pre>  

## Info - Container Engines
<pre>
- is a high-level software that is used to manage containers and images
- it internally uses Container Runtime to manage containers and images
- it is very user-friendly
- examples
  - Docker 
  - Podman
  - containerd
</pre>  

## Container Orchestration Overview
<pre>
- Container Orchestration platform tools helps us manage our containerized application workloads
- instead of manually creating containers and managing them, we could use Container Orchestration Platforms to manage them for us
- Examples
  1. Docker SWARM - Docker's native Container Orchestration Platform
  2. Google Kubernetes - Free & Production grade Container Orchestration Platform
  3. Red Hat Openshift - Enterprise Paid software - Red Hat's Distribution of Kubernetes
- General features supported
  - provides an environment to make your containerized application works Highly Available (HA)
  - based on traffic to one's application individual applications can be scaled up/down on demand manually/automatically
  - upgrading/downgrading your already live applications from one version to the other without any down time using Rolling update
  - exposing your applications within cluster or to external world using services
- it is a self-healing platform
  - it can repair itself to some extent
  - it can repair your application when it is found faulty
</pre>

## Info - Docker SWARM Overview
<pre>
- this is Docker Inc's Container Orchestration Platform
- it only supports managing Docker containerized application workloads
- it is pretty easy to install and learn
- can be installed on a laptop with pretty basic configuation as well as it is very light weight
- good for POC or learning purpose
- not production grade
</pre> 

## Info - Google Kubernetes Overview
<pre>
- free and opensource container orchestration platform developed by Google along with many open source contributors
- it is production grade
- free for personal and commercial use
- as it is opensource, we won't get support from Google
- only supports command line interface (CLI)
- doesn't support web console
- Kubernetes does provide some basic Dashboard but it is considered a security vulnerability, hence no one uses the Kubernetes Dashboard
- Rancher is opensource webconsole for Google Kubernetes
- Kubernetes supports RBAC, Custom Resource Definition (CRD) to extend the Kubernetes API to add new type of resources
- Kubernetes also allows us to develop and deploy custom operators to manage our custom resources
- Kubernetes operators - is a combination of many Custom Resources + Customer Controllers
- supports inbuilt monitoring features
  - it can check the health of our application, when it finds your application is not responding, it can repair it or replace it with another good healthy instance of your application
  - it supports inbuilt load-balancing
- supports any container runtime/engine as long as there is an implemention of CRI(Container Runtime Interface) to interact with those runtime/engine
</pre>  

## Info - OKD
<pre>
- is opensource Container Orchestration Platform maintained by opensource community
- is developed on top of Google Kubernetes
- supports both CLI and webconsole
- comes with in-built internal Image Registry
- you only get community support
- supports user-management
- supports deploying application from source code is called S2I (Source to Image)  
- supports CI/CD
- support Virtualization  
</pre>

## Red Hat Openshift Overview
<pre>
- Red Hat Openshift is developed on top of Opeensource OKD ( which in turn is developed on top of opensource Kubernetes )
- supports command line interface and webconsole(GUI)
- supports Role based access control (RBAC), hence multiple users can be created with different level of access to Openshift cluster
- supports many additional features on top of all the Kubernetes features
  1. User Management
  2. Pre-integrated monitoring tools ( Prometheus & Grafana Dashboards )
  3. Out of the box - Private Container Registry
  4. Routes - a new feature only available in Openshift which is developed on top of Kubernetes Ingress
  5. Using the Kubernetes operators, the Red Hat Openshift team has many additional features on top of Kubernetes
- Openshift version upto 3 - supported many different container runtime/engines including Docker
- Openshift version 4 and above - only supports Podman Container Engine and CRI-O Container Runtime
- Due to security vulnerabilities issues in Docker, 
</pre>

## High-Level Openshift Architecture
![Architecture](openshiftArchitecture.png)
![OpenShift High Level Architecture](master-node.png)

# Info - Pod Overview
<pre>
- Pod is a collection of related containers
- it is configuration object that is stored and maitained within etcd key-value database by API server
- as per recommended industry best practice, one main application per Pod is better
- Pod is the smallest unit that can be deployed within Kubernetes/Openshift
- all the containers within a Pod, shares the IP address, ports, etc.,
- each Pod get a its own dedicated port range 0-65535
</pre>
![pod](pod.png)

## Info - ReplicaSet Overview
<pre>
- it is a configuration object that is stored and maintained within etcd key-value database by API server
- it tells how many instance of a particular Pod type should be running at point of time
- this configuration object is consumed by ReplicaSet controller
- it the ReplicaSet controller which manages the Pod instances
- the ReplicaSet controller is the one which supports scale up/down
</pre>

## Info - Deployment Overview
<pre>
- stateless applications are normally deployed as a deployment
- it is a configuration object that is stored and maintained within etcd database by API server
- it describes
  - name of the application deployment
  - number of Pod instances expected to be running
  - container image and version that must be used to create Pod
- it supports rolling update
- this configuration object is consumed by Deployment Controller
</pre>

## Kube config file
- the oc/kubectl client tools requires a config file that has connection details to the API Server(load balancer)
- the config file is generally kept in user home directory, .kube folder and the default name of kubeconfig is config
- optionally we could also use the --kubeconfig flag with the oc command to point to a config file
- it is also possible to use a KUBECONFIG environment variable to point to the config file
- Just to give an idea, it is possible that your Kubernetes/OpenShift is running in AWS/Azure but you could install oc/kubectl client tool on your laptop with a config file and still run all the oc/kubectl commands from your laptop without going to aws/azure

To print the content of kubeconfig file
```
cat ~/.kube/config
```
## About Red Hat Enterpise Core OS ( RHCOS )
- an optimized operating system created especially for the use of Container Orchestration Platforms
- each version of RHCOS comes with a specific version of Podman Container Engine and CRI-O Container Runtime
- RHCOS enforces many best practices and security features
- it allows writing to only folders the application will has read/write access
- if an application attempts to modify a read-only folder RHCOS will not allow those applications to continue running
- RHCOS also reserves many Ports for the internal use of Openshift
- User applications will not have write access to certain reserved folders, user applications are allowed to perform things as non-admin users only, only certain special applications will have admin/root access

### Points to remember
- Red Hat Openshift uses RedHat Enterprise Linux Core OS
- RHCOS has many restrictions or insists best practises
- RHEL Core OS reserves ports under 1024 for its internal use
- Many folders within the OS is made as ready only
- Any application Pod attempts to perform write operation on those restricted folders will not be allowed to run
- For detailed documentation, please refer official documentation here https://docs.openshift.com/container-platform/4.8/architecture/architecture-rhcos.html

## Info - Pod Lifecycle
- Pending - Container image gets downloads or there are no Persistent Volume to bound and claim them
- Running - The Pod is scheduled to a node and all containers in the Pod are up and running
- Succeeded - All containers in the Pod have terminated succesfully and not be restarted
- Failed - All containers in the Pod have terminated but one or more containers terminated with non-zero status or was terminated by Openshift
- Unknown - For some reason, the state of the Pod could not be obtained may be there is some problem in communicating to the node where the Pod is running

## Info - Container Lifecycle
- Waiting - pulling the container image
- Running - container is running without issues
- Terminated - container in the Terminated state began execution and then either ran to completion or failed for some reason

## Lab - Checking the openshift tool version
```
oc version
kubectl version
oc get nodes
oc get nodes -o wide
```

## Lab - Creating a Pod in docker 
```
docker images
docker pull gcr.io/google_containers/pause-amd64:3.1
docker images

docker run -d --name nginx_pause --hostname nginx gcr.io/google_containers/pause-amd64:3.1
docker run -d --name nginx --network=container:nginx_pause nginx:latest
docker ps
docker inspect nginx_pause | grep IPA
docker exec -it nginx sh
hostname -i
hostname
apt update && apt install net-tools iputils-ping
ifconfig
exit
```

Expected output
![image](https://github.com/user-attachments/assets/1fda8ebd-1cb7-4fda-82b8-734730f8cc0c)
![image](https://github.com/user-attachments/assets/881f4228-bebd-46c0-8910-1523bb1b4ce0)
![image](https://github.com/user-attachments/assets/604206a2-b978-4c04-9033-2a4eb9c7fa1f)
![image](https://github.com/user-attachments/assets/015ee964-997c-45ca-96b6-5d25a412f998)

## Info - Kubernetes/Openshift Control Plane Components
The control plane components only runs in master node
<pre>
1. API Server
2. etcd database
3. Controller Managers
4. Scheduler
</pre>

#### API Server
<pre>
- API Server is the heart of Kubernetes/Openshift
- supports REST API for all the features supported by OpenShift
- all the control plane components interacts only with API Server by making a REST call
- API Server stores/updates/retrieves the cluster status, application status into etcd database
- each change that is done in the etcd database will result in API Server raising events
- all the Openshift components will be communicate only to API Server
- other components are not allowed to communicate with each other directly
- every components communication flows via API Server only in Kubernetes/Openshift
- API Server maintains the nodes, cluster, application status in the etcd database
- only API Server will have access to etcd database
- In our openshift cluster, 3 master nodes are there, hence 3 API Servers i.e one API Server per master node is there
- API Server sends broadcasting events whenever any update happens in the etcd database
  - new record added
  - existing record updated
  - existing record deleted
</pre>

#### Etcd database
<pre>
- key/value database
- it is an independent opensource database used in Kubernetes & Openshift
- generally etcd database works as a cluster i.e group of etcd instances works together as a cluster
- when one of the etcd db instance is updates, the data is synchronized automatically on other etcd instances within the etcd cluster
- minimum number of instances required to create an etcd cluster is 3
</pre>

#### Controller Managers
<pre>
- it is a collection of many controllers
- Controllers are deployed as Pods
- Each Controller watches the cluster
  - when new resources are created
  - when existing resources in etcd are edited/updated
  - when existing resource are deleted from etcd
- Based on the events from API server the Controller job to reconcile based on updated configuration objects
- Some controllers within Controller Manager Pod
  - Deployment Controller
  - ReplicaSet Controller
  - EndPoitnt Controller
  - Job Controller
  - StatefulSet Controller
  - DaemonSet Controller
- Each Controller manages one type of Kubernetes/OpenShift resource
- For example
  - Deployment Controller manages Deployment resource
- Deployment Controller watches for events related to Deployment Resource
  - New deployment created
  - Depoyment edited
  - Deployment deleted
- ReplicaSet Controller watches for events from API Server related to ReplicaSet Resource
  - New ReplicaSet created
  - ReplicaSet edited
  - ReplicaSet deleted
</pre>  

#### Scheduler
<pre>
- this is the component that is responsible to find a healthy node where a new Pod can be deployed
- Scheduler sends its scheduling recommendataion to API Server
- API Server updates the scheduling info on each Pod stored in the etcd database
- API Server broadcasts events for each Pod deployed onto some node
- Kubelet is the container agent that runs in every node ( master and worker nodes)
- kubelet downloads the container image, creates and starts the container
- kubelet keeps monitoring the status of the container running on the local node and reports the status on a heart-beat like periodic fashion to the API Server
</pre>  

## Info - Understanding Red Hat Openshift installation
<pre>
- Assume we wish to have 3 master nodes with 3 worker nodes
- each of these nodes can be a Virtual Machine in on-prem server or it could be a physical in your office or it could be an ec2 in aws or it could be an azure vm in azure cloud
- OpenShift master nodes will accept only Red Hat Enterprise Core OS as the Operating System
- The Red Hat Enterprise Core OS  (RHCOS)
  - it comes Podman Container Engine and CRI-O Container Runtime pre-installed
- Openshift worker nodes has opt for either Red Hat Enterprise Linux (RHEL) or Red Hat Enterprise Core Linux (RHCOS)
- Bastion Virtual Machine (Helper Virtual Machine)
  - Can be any Linux OS
  - In our lab, I have installed Fedora 39 Linux
  - This is where we will install DNS Server
  - This is where we will install DHCP Server
  - This is where we will install HAProxy Load Balancer server
  - The HA Proxy Load Balancer will host the following
    - Red Hat Enterprise Core OS Image
    - Red Hat Openshift master node ignition file
    - Red Hat Openshift worker node ignition file
    - Red Hat Openshift bootstrap node ignition file
- BootStrap Virtual Machine ( Virtual Machine - used to setup Openshift master and worker nodes )
  - Red Hat Enterprise Core OS
  - In order to install openshift, the openshift installer creates a simple Kubernetes cluster within BootStrap Virtual Machine
  - The BootStrap Virtual Machine - Kubernetes cluster installs the required openshift master node components into master-1, master-2 and master-3 VMs
  - Once the Kubernetes cluster running in BootStrap VMs finds the master-1, master-2 and master-3 nodes in openshift are found to be stable, the Kubernetes cluster running in BootStrap VM is no more required
  - The sole purpose of BootStrap Virtual Machine is to create a 3 node master cluster in Openshift
  - In most case, people prefer disposing this VM once the 3 openshift master nodes are up and running as a cluster
- Master Node 1
  - We need to Red Hat Enterprise Core OS as an Operating Sytem
  - RHCOS comes with 
    - preinstalled Podman Container Engine and CRI-O container runtime
  - Control Plane Components will be running
    - API server (Pod)
    - etcd database (Pod)
    - controller managers (Pod)
    - scheduler (Pod)
- Master Node 2
  - We need to Red Hat Enterprise Core OS as an Operating Sytem
  - RHCOS comes with 
    - preinstalled Podman Container Engine and CRI-O container runtime
  - Control Plane Components will be running
    - API server (Pod)
    - etcd database (Pod)
    - controller managers (Pod)
    - scheduler (Pod)
- Master Node 3
  - We need to Red Hat Enterprise Core OS as an Operating Sytem
  - RHCOS comes with 
    - preinstalled Podman Container Engine and CRI-O container runtime
  - Control Plane Components will be running
    - API server (Pod)
    - etcd database (Pod)
    - controller managers (Pod)
    - scheduler (Pod)
  - kube-proxy (Pod)
  - kubelet (service)
  - coreDNS (Pod)
- Worker Node 1
  - We need to Red Hat Enterprise Core OS as an Operating Sytem
  - RHCOS comes with 
    - preinstalled Podman Container Engine and CRI-O container runtime
  - kube-proxy (Pod)
  - kubelet (service)
  - coreDNS (Pod)
- Worker Node 2
  - We need to Red Hat Enterprise Core OS as an Operating Sytem
  - RHCOS comes with 
    - preinstalled Podman Container Engine and CRI-O container runtime
  - kube-proxy (Pod)
  - kubelet (service)
  - coreDNS (Pod)
- Worker Node 3
  - We need to Red Hat Enterprise Core OS as an Operating Sytem
  - RHCOS comes with 
    - preinstalled Podman Container Engine and CRI-O container runtime
  - kube-proxy (Pod)
  - kubelet (service)
  - coreDNS (Pod)
</pre>

## Info - Openshift client tools
<pre>
oc - is the command-line openshift client
kubectl - is the kubernetes command-line client
both oc and kubectl are supported in openshift cluster
</pre>

### OpenShift resources
- Deployment (K8s resource)
- ReplicaSet (K8s resource)
- Pod (K8s resource)
- Job (K8s resource)
- DaemonSet (K8s resource)
- StatefulSet (K8s resource)
- Build ( OpenShift resource - Custom Resource added by OpenShift )
- ImageStream ( OpenShift resource  - Custom Resource added by OpenShift ) 
- DeploymentConfig ( OpenShift resource - Custom Resource added by OpenShift )

### Deployment command looks like this
```
oc create deployment nginx --image=bitnami/nginx:latest --replicas=3
```

### Deployment
- This is a JSON/YAML definition which is stored in etcd database
- The deployment is managed by Deployment Controller
- when we applications, they are deployed as Deployment with Kubernetes/OpenShift
- Deployment Controller creates ReplicaSet, which is then managed by ReplicaSet Controller
- Deployment has one or more ReplicaSet(s)

### ReplicaSet
- This is a JSON/YAML definition which is stored in etcd database
- The ReplicaSet is managed by ReplicaSet Controller
- ReplicaSet capture details like
    - How many Pod instances are desired?
- ReplicaSet Controller reads the ReplicaSet definition and learns the desired Pod instance count
- ReplicaSet Controller creates so many Pod definition as indicated in the ReplicaSet
- ReplicaSet Controller ensures the desired Pod count matches with the actual Pod count, whenever a Pod crashes, it is the responsibility of ReplicaSet Controller to ensure the desired and actual Pods are equal
- ReplicaSet has one or more Pods

### Pod
- is a collection of one or more Containers
- IP address is assigned on the Pod level not on the Container level
- If two containers are in the same Pod, there will be sharing IP Address of the Pod
- within container, application are deployment ( tomcat,mysql, nginx these are applications )
- recommended best practice,only one application should be there in a Pod
- Pods are scheduled by Scheduler onto some Node
- every Pod has a Network Stack and Network Interface Card (NIC)

### Kubelet
- is a daemon service that interacts with the Container Runtime on the current node/server where kubelet is running
- kubelet downloads the required container image and creates the Pod containers
- kubelet frequently reports the status of Pod container status to the API server
- kubelet also monitors the health of POds running on the node and ensures they are healthy
- kubelet will there on every node ( master and worker nodes )

### kube-proxy
- is a Pod that runs one instance per node (both master and worker nodes)
- provides load-balancing a group of similar Pods

### Core DNS
- is a Pod that runs usually on master nodes but in some installation, it might even run on worker node
- Core DNS offers Service Discovery i.e application will be connect to group of Pods by Service name

### Kubectl
- is a client tool used to create and manage deployments and services in Kubernetes
- it also works in OpenShift
- it make REST call to API Server

### OC
- is a client tool used to create and manage Openshift resources in OpenShift
- it makes REST call to API Server
