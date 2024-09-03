# Day 2

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
