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
