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
