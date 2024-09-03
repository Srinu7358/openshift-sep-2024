![image](https://github.com/user-attachments/assets/d9de9a1a-8bdc-468a-b23e-553e87bdf3d1)# Day 2

## Info - Openshift Control Plane Components ( Runs only in master nodes )
<pre>
- API Server Pod
- etcd database Pod
- controller managers Pod
- scheduler Pod
</pre>

## Lab - List all the openshift nodes ( as non-admin user )
```
oc get nodes
oc get nodes -o wide
oc version
kubectl version
```

Expected output
![image](https://github.com/user-attachments/assets/d51dde4c-3386-4ac1-8884-6918b450023b)
![image](https://github.com/user-attachments/assets/44abd122-e018-4cba-81cb-961082a8885c)

## Lab - List all API Servers from all the master nodes present in the openshift cluster
```
oc get pods -n openshift-kube-apiserver -o wide
```

Expected output
![image](https://github.com/user-attachments/assets/c1c13c1b-de04-41f7-8e27-9c3d8931a214)

## Lab - List all etcd pods from all master nodes
```
oc get pods -n openshift-etcd
```
Expected output
![image](https://github.com/user-attachments/assets/aad203b4-9ac6-4f98-a6a2-63ba32e7677b)

## Lab - List all controller manager pods from all master nodes
```
oc get pods -n openshift-kube-controller -o wide
```

Expected output
![image](https://github.com/user-attachments/assets/b875781a-0101-49a5-af29-c7f9f615e71f)

## Lab - List all scheduler pods from all master nodes
```
oc get pods --all-namespaces | grep scheduler
oc get pods -n openshift-kube-scheduler
```

Expected output
![image](https://github.com/user-attachments/assets/8d96f10e-e6b0-40e5-a050-ee990f015c91)

## Lab - Listing all namespaces in openshift
<pre>
- namespace/project is a way to segregate application deployments done by one user/team from other users/teams
- administrators can control which users has access to specific projects
</pre>
```
oc get namespaces
oc get projects
```
Expected output
![image](https://github.com/user-attachments/assets/5b65abc5-ba0d-4f45-b808-624c5fa28fe1)
![image](https://github.com/user-attachments/assets/4fedb351-0daf-402e-9524-9e1a10c10963)

## Lab - Creating a project for your application deployments
In the below command, replace 'jegan' with your name
```
oc new-project jegan
oc get projects | grep jegan
```

Expected output
![image](https://github.com/user-attachments/assets/ae696f69-5650-4464-bf08-09ea3e16b8a2)

## Lab - Switching between projects
```
oc project
oc project default
oc project
oc project jegan
```

Expected output
![image](https://github.com/user-attachments/assets/60130e0c-79b5-49d2-a9aa-93f7d77a653a)

## Lab - Finding you logged in as which user within openshift
```
oc whoami
oc whoami --show-server
oc whoami --show-console
```

Expected output
![image](https://github.com/user-attachments/assets/3280ffc6-b5dc-414d-88c7-d8a30f14f854)

## Lab - Find more details about a node
```
oc describe node/master01.ocp4.rps.com
oc describe node/master02.ocp4.rps.com
oc describe node/master03.ocp4.rps.com
oc describe node/worker01.ocp4.rps.com
oc describe node/worker02.ocp4.rps.com
```
Expected output
![image](https://github.com/user-attachments/assets/0562df6c-6505-44a0-b5e0-fe012bae589b)

## Lab - Deploying your first application within your project namespace
```
oc project jegan
oc create deployment nginx --image=nginx:latest --replicas=3
```

Listing the deployments
```
oc get deployments
oc get deployment
oc get deploy
```

Expected output
![image](https://github.com/user-attachments/assets/8ebda529-ec89-4994-8bde-a8efe6dd8657)

Listing the replicasets
```
oc get replicasets
oc get replicaset
oc get rs
```

Expected output
![image](https://github.com/user-attachments/assets/b5ac4a3e-79ff-49b9-9320-f795b1f05564)

Listing all the pods in your project namespace
```
oc get pods
oc get pod
oc get po
```
Expected output
![image](https://github.com/user-attachments/assets/1c50ef46-d4bf-4518-9097-93219a0659ff)

## Lab - Troubleshooting - checking pod logs
```
oc get po
oc logs pod/nginx-56fcf95486-mpb4m
```

Expected output
![image](https://github.com/user-attachments/assets/4e186163-671e-4e91-b9ad-f3be4ff9e2fe)

#### Things to note
<pre>
- Openshift will not allow regular applications to run administrative tasks
- in the above logs, note that nginx.conf at line number 2, it seems to create a folder outside the home directory, which can be done only by an administrator
- as the application is running as non-admin user, due to permission issues the Pod is crashing
- the root cause is, the nginx:latest docker image doesn't seem to be following openshift standards, hence this kind of docker images must be avoided in openshift
- instead we could use bitnami/nginx:latest docker image that meets all the openshift best practices
</pre>


## Lab - Deleting a deployment under your project
```
oc project
oc get deploy
oc delete deploy/nginx
oc get deploy,rs,po
```

Expected output
![image](https://github.com/user-attachments/assets/d074baaa-4b6e-4c16-9fd1-bd229848cb1a)

## Lab - Deploying nginx web server using bitnami docker image
```
oc project
oc create deployment nginx --image=bitnami/nginx:latest --replicas=3
oc get deploy,rs,po
```

Expected output
![image](https://github.com/user-attachments/assets/c9a184de-e46b-4d34-9a7f-dcd806bb7b1c)
![image](https://github.com/user-attachments/assets/56e188e1-41de-4779-bb0f-cb19376d32c8)

#### Things to note
<pre>
- bitnami images generally follows openshift or general industry best practices, hence almost all bitnami containers images are openshift compatible
- bitnami images also ensure that ports below 1024 aren't used as they are reserved by openshift for its internal use
- bitnami images are rootless, i.e they perform everything as non-admin users
</pre>

## Lab - Scaling up or down the pod instances count(replicas) in a deployment

To come out of the watch mode,you need to press Ctrl + C
```
oc get deploy,rs,po
oc scale deploy/nginx --replicas=5
oc get po -o wide -w
oc scale deploy/nginx --replicas=2
oc get po 
```

Expected output
![image](https://github.com/user-attachments/assets/626fc2d7-9109-4656-b1db-194c2f885ac3)

## Lab - Finding more details about a deployment
```
oc get deploy
oc describe deploy/nginx
```

Expected output
![image](https://github.com/user-attachments/assets/022cc5db-2ba5-4e68-a7dd-a814393dbc91)
![image](https://github.com/user-attachments/assets/07556ac4-0e9e-4a2c-b962-43cc7986171d)

## Lab - Finding more details about a replicaset
```
oc get rs
oc describe rs/nginx-
```

Expected output
![image](https://github.com/user-attachments/assets/4124ad1e-fc36-4257-9897-a86c5840d73e)

## Lab - Finding more details about a pod
```
oc get po
oc describe pod/
```

Expected output
![image](https://github.com/user-attachments/assets/92a76736-a100-4999-9ad2-5bb80e55c0bc)
![image](https://github.com/user-attachments/assets/fc73870e-fe2b-4569-bef5-d936113a01d6)
![image](https://github.com/user-attachments/assets/1fe2b791-efb5-4903-a785-f53d261b2d5c)

## Lab - Getting inside a Pod shell
```
oc get po
oc rsh deploy/nginx
hostname
hostname -i
exit
oc exec -it pod/nginx-aabbccdd-1awefa
exit
```

Expected output
![image](https://github.com/user-attachments/assets/c32a4795-180a-4c6c-8829-d03dd1289a53)

## Lab - Finding a pod IP address
```
oc get po -o wide
```

Expected output
![image](https://github.com/user-attachments/assets/c16bd189-6555-4078-b9f8-ff4a978227e0)

## Lab - Edit a deployment specification
Once you edit, you could update the replicas to 5, save and exit the file.
```
oc get deploy
oc edit deploy/nginx
oc get po -w
oc get po
```

Expected output
![image](https://github.com/user-attachments/assets/95a3e4dd-7dca-4fb9-96b2-0886477a8536)
![image](https://github.com/user-attachments/assets/32d7e1f4-54f9-49cf-8a18-bb3f79189910)
![image](https://github.com/user-attachments/assets/61488cf1-569f-4549-a878-ae515caf6c6a)

## Lab - Edit a replicaset definition
```
oc get rs
oc edit rs/nginx-6677c75969
```

Expected output
![image](https://github.com/user-attachments/assets/5be83c26-e010-4645-9fd8-faf95f86ca2b)
![image](https://github.com/user-attachments/assets/7a426c4e-ec37-4ad2-a238-d44e7009c67d)

## Lab - Editing a pod definition
```
oc get po
oc edit pod/nginx-aabbccdd-3453asf
```

Expected output
![image](https://github.com/user-attachments/assets/c29d24ab-fe86-42d2-a6b0-a95275c4b80d)
![image](https://github.com/user-attachments/assets/4845d068-6424-4101-bd10-22797ee07857)
![image](https://github.com/user-attachments/assets/a000dc47-f0bd-46d4-a14e-0d58ead5b00e)

## Lab - Testing a Pod using port forwarding ( Not used in production )
Run the below command in one terminal tab
```
oc port-forward pod/nginx-aabcadsf-r4acs34 9005:8080
```
Expected output
![image](https://github.com/user-attachments/assets/bed58dca-1133-4403-884d-bb9592880f47)

From another terminal tab, you can access the web page served the above pod as shown below
```
curl http://localhost:9005
```
Expected output
![image](https://github.com/user-attachments/assets/141c9150-7721-4269-a400-cdc18f453563)

In order to quit the port-forward, go back the first tab and press Ctrl+C

#### Things to note
<pre>
- port-forward is used primarily for testing purpose
- this is never used in production
- this is just a quick way to test if the application running within a Pod container is working fine
- hence must be avoid otherwise
- the port on the left side is opened up on your local machine
- the port on the right side 8080 is used by bitnami/nginx container within the Pod
- you can use any port which is not already used by some participant on that server, instead of 9005 you may try any other port 
</pre>

## Info - Openshift/Kubernetes service
<pre>
- service represents a group of load-balanced pods from a single deployment
- service can be given an unique name, it acquires an unique ip address which are stable
- anyone who wants to access the application Pods, they can access them via service name or its IP address
- services are of 2 types
  1. Internal service ( accessible within openshift cluster - i.e pods )
     - ClusterIP service
  2. External service ( accessible outside openshift cluster )
     - NodePort service
     - LoadBalancer service
</pre>

## Info - ClusterIP Service Overview
<pre>
- is an internal service
- accessible only within the Pod shell, i.e inside openshift cluster only
- practical use-cases for ClusterIP service is to expose database for front-end application/microservice running with openshift
- internal load-balancing done kube-proxy pod that runs in every kubernetes/openshift node
</pre>

## Info - NodePort Service Overview
<pre>
- is an external service
- accessible outside openshift cluster
- Kubernetes/Openshift has reserved ports in the range 30000-32767 for the purpose of node port services
- For each NodePort service we create for a deployment, a port in the range 30000-32767 will be allocated for the nodeport service
- the nodeport is opened by openshift in all the nodes present in the openshift cluster (master1,master2,master, worker1 and worker2)
- internal load-balancing done kube-proxy pod that runs in every kubernetes/openshift node  
- advantages of using NodePort service
  - it is an internal implementation of Kubernetes/Openshift, hence even if you run your cluster in AWS/Azure it won't attract any extra charges when we create a NodePort service
- drawbacks
  - it is not user-friendly to access a node-port, it is not convenient
  - we will end up opening many ports in firewall for each node port service we create, this might attract security issues
</pre>

## Info - LoadBalancer Service Overview
<pre>
- is an external service
- generally used in public cloud environments like AWS, Azure, GCP, Digital Ocean, etc.,
- can also be used in On-Prem Kubernetes/Openshift setup like our lab setup
- in order to support load-balancer service in a local openshift setup, we need to install Metallb Operator and configure it
- this will create an external loadbalancer like ELB/ALB etc., in public cloud environments like AWS/Azure
- advantages
  - it is convenient to access as they tend to us user-friendly ports like 8080, etc.,
  - user-friendly
  - it is free, when it is used in a local Kubernetes/openshift setup with Metallb operator
- drawbacks
  - it will attract extra charges when used in EKS ( AWS Elastic Kubernetes Service - Managed Kubernetes cluster in AWS )
  - it will attract extra charges when used in AKS ( Azure Kubernetes Service - Managed Kubernetes cluster in Azure )
  - it will attract extra charges when used in ROSA ( AWS Red Hat Openshift - Managed Openshift cluster in AWS )
  - it will attract extra charges when used in ARO ( Azure Red Hat Openshift - Managed Openshift cluster from Azure )
</pre>

## Info - Ingress
<pre>
- Ingress is a forwarding rule
- it is not a service
- it is a feature implemented by Kubernetes, also supported in Openshift
- For Ingress to work, we need the below in Openshift/Kubernetes Cluster
  - Load Balancer ( e.g: Nginx LB or HA Proxy, etc )
  - Ingress Controller ( e.g Nginx Ingress Controller or HAProxy Ingress Controller )
  - Ingress ( Forwarding rule - user defined )
- Ingress can forward the calls to multiple different services based on rules we define
- Ingress provides a convenient public url to access the application externally
</pre>  

## Info - Routes
<pre>
- Route is a feature introducted in Red Hat Openshift
- not supported in Kubernetes
- Route is implemented based on Kubernetes Ingress
- Route generally forward the call to a single Kubernetes/Openshift Service ( ClusterIP, NodePort or LoadBalancer service )
- Route provides a convenient public url to access the application externally
- gives the benefit of loadbalancer service without attracting any extra charges
- in general, openshift users don't use nodeport service at all instead they use Route
- Route is also a free alternate to LoadBalancer service
</pre>

## Lab - Creating an internal clusterip service in imperative style
```
oc get deploy
oc expose deploy/nginx --type=ClusterIP --port=8080
oc get services
oc get service
oc get svc
```
Expected output
![image](https://github.com/user-attachments/assets/a0646605-3ccc-477c-a124-e08ee4f2b3a3)

You can find more details about a service as shown below
```
oc describe svc/nginx
```

Expected output
![image](https://github.com/user-attachments/assets/16339383-6ab3-4b7e-a46b-c077fb191285)

To access the nginx clusterip internal service, let's create a test-pod use service discovery( accessing service by its name )
```
oc run test-pod --image=tektutor/spring-ms:1.0
oc get po
oc exec -it pod/test-pod sh
curl http://nginx:8080
curl http://<service-name>:<service-port>
exit
```

In the above command
<pre>
nginx - is the name of the nginx clusterip service
8080 - is the service port
</pre>

Expected output
![image](https://github.com/user-attachments/assets/f2608ce4-c4b0-40c2-8527-d2daf8107f52)
![image](https://github.com/user-attachments/assets/d8d43e97-0a57-42c2-8839-6861729b6e62)

You could also access the clusterip internal service as shown below
```
oc exec -it pod/test-pod sh
curl http://172.30.40.10:8080
curl http://<service-ip>:<service-port>
```

Expected output
![image](https://github.com/user-attachments/assets/a87593d4-7ecf-405d-a9dd-10779d05c146)
![image](https://github.com/user-attachments/assets/91ed17d6-33ac-4e73-89d3-d977272a1ecf)
![image](https://github.com/user-attachments/assets/b1310035-a2b0-43f9-9581-ee69cbec68cb)
![image](https://github.com/user-attachments/assets/eefe9d14-4532-47fc-825f-8a79fa2102a1)

## Lab - Creating an external nodeport service for nginx deployment
We need to delete the existing clusterip internal service
```
oc get svc
oc delete svc/nginx
```

Let's create an extenal nodeport service for nginx deployment
```
oc expose deploy/nginx --type=NodePort --port=8080
oc get svc
oc describe svc/nginx
curl http://192.168.100.11:32732
curl http://192.168.100.12:32732
curl http://192.168.100.13:32732
curl http://192.168.100.21:32732
curl http://192.168.100.22:32732
```

Expected output
![image](https://github.com/user-attachments/assets/1ad870b8-0737-4bd2-b3b2-1835ce9f6b01)
![image](https://github.com/user-attachments/assets/8a92da85-bb53-49e9-a4d2-67156391ca4c)
![image](https://github.com/user-attachments/assets/4da88f0a-bed9-46a1-9877-33890ae81d0b)
![image](https://github.com/user-attachments/assets/ce56caa1-ed0a-4f23-9850-1bb008494a8d)
![image](https://github.com/user-attachments/assets/2ac83374-27c4-4805-82b6-acbb6d4a1527)
![image](https://github.com/user-attachments/assets/83e5915e-a0c5-4670-884b-4664b99e6dd4)
![image](https://github.com/user-attachments/assets/8733ab20-258b-498f-bf0c-cd143d86f5b8)
![image](https://github.com/user-attachments/assets/036f216f-90be-473c-b7ef-50483d2f5060)
![image](https://github.com/user-attachments/assets/f998e534-5386-41d4-a140-f39c5cc00b63)

## Demo - Installing Metallb operator

You may refer my medium blog for step by step instructions here
<pre>
https://medium.com/tektutor/using-metallb-loadbalancer-with-bare-metal-openshift-onprem-4230944bfa35  
</pre>

## Lab - Creating an external LoadBalancer service for nginx deployment
Let's delete the existing nodeport service
```
oc get svc
oc delete svc/nginx
```

Let's create the loadbalancer external service
```
oc expose deploy/nginx --type=LoadBalancer --port=8080
oc get svc
oc describe svc/nginx
```

Expected output
![image](https://github.com/user-attachments/assets/840d9456-6fa7-4115-bbf1-b6b4a2b0fb44)

LoadBalancer service is supposed to acquire an external IP automatically. Since, we are using a local openshift setup, it won't happen automatically. We need to install Metallb operator and configure to get it to working.
![image](https://github.com/user-attachments/assets/5dc6b1a4-ed6a-4c42-822b-bdb5ea30bb0f)

Let's configure the address pool, i.e reserve some range IP addresses for openshift to assign to the load balancer created each time we create a load balancer service.

Clone the TekTutor openshift Training repository
```
cd ~
git clone https://github.com/tektutor/openshift-sep-2024.git
cd openshift-sep-2024
git pull
cd Day2/metallb
cat addresspool.yml
cat metallb.yml
```

Expected output
![image](https://github.com/user-attachments/assets/ada53054-b5e0-4f46-a465-c135072e0aab)

Let's reserve the address pool range for Metallb operator's use
```
cd ~/openshift-sep-2024
git pull
cd Day2/metallb
oc apply -f addresspool.yml
oc apply -f metallb.yml
```

Expected output
![image](https://github.com/user-attachments/assets/9832fc5b-cea5-40db-9e4a-f57d8a019acb)

Let's check if the loadbalancer service has acquired an external IP address from the reserved address pool we configured
```
oc get svc
curl http://192.168.100.90
```

## Lab - Labels are used as a selector 
```
oc get deploy --show-lables
oc get deploy -l app=nginx

oc describe deploy/nginx
oc get rs --show-lables
oc get rs -l app=nginx

oc get po --show-lables
oc get po -l app=nginx
```

Expected output
![image](https://github.com/user-attachments/assets/bf8a09bb-a855-454a-85f5-0310796ec78c)
![image](https://github.com/user-attachments/assets/5fa39f5f-7422-4d60-857e-b2954053ad85)
![image](https://github.com/user-attachments/assets/2d432b22-8e39-494e-889d-75c663173094)
![image](https://github.com/user-attachments/assets/b8b35c90-b000-404f-a88f-6726d9ad0c81)
![image](https://github.com/user-attachments/assets/2f8ceef2-cb79-4c0a-9f73-f4bfe25bc83d)
