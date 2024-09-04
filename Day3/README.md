# Day 3

## You may like these articles - go thru when you find time
<pre>
https://mvallim.github.io/kubernetes-under-the-hood/documentation/kube-flannel.html

https://docs.tigera.io/calico/latest/reference/architecture/overview

https://yuminlee2.medium.com/kubernetes-weave-net-cni-plugin-810849203c73
  
https://www.techtarget.com/searchitoperations/tutorial/Step-by-step-guide-Get-started-with-Weave-for-Kubernetes#:~:text=Weave%20Net%2C%20often%20simply%20called,to%20talk%20to%20each%20other.

https://kubernetes.io/docs/tasks/administer-cluster/network-policy-provider/weave-network-policy/

https://granulate.io/blog/kubernetes-architecture-beginner-guide/

</pre>

## References - Must read to understand Kubernetes/Openshift Network Model
<pre>
https://www.tkng.io/

https://www.caseyc.net/cni-talk-kubecon-18.pdf

https://www.altoros.com/blog/kubernetes-networking-writing-your-own-simple-cni-plug-in-with-bash/

https://logingood.github.io/kubernetes/cni/2016/05/14/netns-and-cni.html

https://logingood.github.io/kubernetes/cni/2016/05/15/bagpipe-gobgp.html

https://dougbtv.com/nfvpe/2017/06/22/cni-tutorial/
</pre>

## Info - What happens when we deploy an application in Openshift with the below command
```
oc create deployment nginx --image=bitnami/nginx:latest --replicas=3
```

<pre>
- the oc client tool makes a REST call to API Server requesting it to create nginx deployment with image bitnami/nginx:latest with 3 Pods in it
- API Server receives the request from oc client tool, it then create a new Deployment record in etcd database
- API Server then sends a broadcasting event saying a New Deployment is created
- the event is received by Deployment Controller, it then sends a REST API call to API Server, requesting it to create a ReplicaSet for ngin deployment
- API Server receives the request from Deployment Controller and it creates a ReplicaSet record in etcd database
- API Server sends a broadcasting event saying a New ReplicaSet is created
- the event is received by ReplicaSet Controller, it then understands 3 Pods are mentioned in the Desired count, hence it makes REST call to API server to create 3 Pods
- the API Server creates 3 Pod records in etcd database and it sends broadcasting events say new Pod created.  One such event will be broadcasted for every New Pod created.
- the scheduler receives the event and it sends scheduling recommendation for each Pod to the API Servers
- API Server receives the REST call from Scheduler, it then retrieves the existing Pod records from etcd and it updates the Pod records with the node details as recommended by Scheduler
- API Server then sends broadcasting events that Pod scheduler to so an do nodes
- the kubelet container agent that runs in node where the Pod is scheduled receives the event, it then downloads the container images, creates the container and starts the container
- the kubelet then sends the status of the container running on that nodes to API Server via REST calls
- the API Servers updates the status of the Pod based on the status it received from kubelet
- the kubelet keeps sending this kind of container status updates to API Server like a heart beat fashion
- the API keeps the Pod status updated based on the status reported by kubelet
</pre>

## OpenShift Operators
<pre>
- automating the human operators specific skill within OpenShift cluster
- Operators = Custom Resource(s) + Custom Controller(s)
- One Controller manages one type of Resource
- For example:
     - Deployment Controller manages Deployment
     - ReplicaSet Controller manages ReplicaSet
</pre>    
## How Custom Resource(CR) are added to OpenShift cluster?
<pre>
- We need to define Custom Resource Definition(CRD)
- CRDs introduce/register a new type of Custom Resource(CR) to your OpenShift cluster
- CRDs themselves doesn't add a Custom Resource(CR), we need to create them ourselves
</pre>
## What is an OpenShift Controller?
<pre>
- it is an application that runs within the OpenShift cluster that monitors a specific type of Resource
- whenever it detects a Resource managed by the Controller is added/edited/deleted/updated, it acts
- Controller does 3 things
    1. monitors a specific Resource (Observe/Watch)
    2. compares desired vs current status
    3. Whenever there is a deviation of current status from desired state, Controllers act to ensure Current state matches the actual State
    
Example:
- ReplicaSet Controller monitors the ReplicaSet resources constantly
- ReplicaSet Controller registers with the APIServer ( informers ) for CRUD ( Create, Read, Update and Delete ) events of ReplicaSet
- ReplicaSet Controller will act whenever it sees the Desired Pods doesn't match the current no of Pods
</pre>

## Lab - Getting inside an etcd pod and understand how etcd stores deployment, replicaset, pods, etc
```
oc get pods -n openshift-etcd
oc exec -it 
```

Expected output
![image](https://github.com/user-attachments/assets/0c020429-49a8-40de-9f20-497a004f5247)
![image](https://github.com/user-attachments/assets/45c36b52-b585-4e25-b48e-dfb2a6db0380)
![image](https://github.com/user-attachments/assets/30f47be1-547f-4776-b81f-d6a8cc52415a)


## Lab - Deploying nginx in declarative style

First of all, let's delete the existing deployments, services and routes within our project
```
oc get deploy,svc,routes
oc delete deploy/nginx deploy/hello svc/nginx svc/hello route/nginx route/hello
oc get all
```

Expected output
![image](https://github.com/user-attachments/assets/6b6011d3-b2ef-495b-9985-580c7044ef25)

Let's generate the nginx deployment declarative script
```
oc create deployment nginx --image=bitnami/nginx:latest --replicas=3 -o yaml --dry-run=client
oc create deployment nginx --image=bitnami/nginx:latest --replicas=3 -o yaml --dry-run=client > nginx-deploy.yml
cat nginx-deploy.yml
```

Expected output
![image](https://github.com/user-attachments/assets/8112278c-f6b5-404b-8701-09aed613006c)
![image](https://github.com/user-attachments/assets/0d5962e3-32bd-4c8b-8e6c-e625234f4cd7)

Let's create the nginx deployment in declarative style
```
oc create -f nginx-deploy.yml --save-config
```

Expected output
![image](https://github.com/user-attachments/assets/f36dd645-13c5-417f-a65a-9e0972f6ea8d)

## Lab - Scale up nginx deployment in declarative style
Let's update the nginx-deploy.yml replicas from 3 to 5 and save it before applying the changes into openshift cluster
```
cd ~/openshift-sep-2024
git pull
cd Day3/declarative-manifest-scripts
vim nginx-deploy.yml
oc apply -f nginx-deploy.yml
oc get po -w
oc get po
```

Expected output
![image](https://github.com/user-attachments/assets/75d8b1e3-4345-4f76-9335-ae7e221ef0e9)
![image](https://github.com/user-attachments/assets/92bda2bd-cada-42d4-b7be-724aaa0952e0)
![image](https://github.com/user-attachments/assets/73f152c6-3a5d-4654-85ae-8c1199a0052d)

## Lab - Delete a deployment in declarative style
```
oc get deploy,rs,po
oc delete -f nginx-deploy.yml
oc get deploy,rs,po
```

Expected output
![image](https://github.com/user-attachments/assets/9326324c-8892-4ceb-80b4-fb20e095e3c7)
