# Day 4

## Info - Persistent Volume Overview
<pre>
- any application that stores/retrieves data they could either use the Pod storage or external storage
- as the Pod life time is short, storing the permanent data in a short lived Pod doesn't sound correct
- hence, it is considered a bad practice to modify a pod once it is created
- as per devops philosophy, we must use containers, Pods like an immutable(read-only) resource, though it is mutable
- hence, we should be using some external storage to persistent our application logs, database, etc
- Openshift Administrators can provision external storage either manually or dynamically
- In case the Administrators prefer povisioning the Persistent Volume(storage) manually, they need to create PV with various size, access permissions, etc as per dev/qa/prod requirement
- The Persistent Volume (PV -storage) can be provisioned
  - from NFS Server ( we would be using NFS Server )
  - Some storage solution in on-prem setup
  - could AWS S3 buckets, AWS EBS, Azure Storage, etc.
- the Persistent Volume will have the below parameters
  - disk size
  - access mode
  - storageclass(optional)
  - labels(optional)
- Persistent volumes are created in the cluster scope
</pre>

## Info - Persistent Volume Claim Overview
<pre>
- application Pods are created in a project scoped, hence the PVC is also created within a project 
- any application (Pod) that needs storage will have ask Openshift by creating a Peristent Volume Claim
- the PVC will define the below
  - size of the storage required
  - access mode expected
  - storageclass(optional)
  - any label restrictions (optional)
- openshift will search the cluster for a matching Persistent Volume and if it finds a matching PV, it let's your application claim and use it
</pre>

## Lab - Deploying multipod application Wordpress with MariaDB database
```
cd ~/openshift-sep-2024
git pull
cd Day3/persistent-volume/wordpress
ls
./deploy.sh
```
Expected output
![image](https://github.com/user-attachments/assets/3c0a274b-0a2d-48fd-83c3-7485e394d7bc)
![image](https://github.com/user-attachments/assets/17a02d4c-b4f1-423a-971f-38eba05be651)
