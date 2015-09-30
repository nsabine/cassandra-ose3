##Making This Example Work in OSE3

1.) Clone the repository into an OSE3 user's working directory
```console
git@github.com:kevensen/cassandra-ose3.git
```

2.) Login to OSE3
```console
oc login
```

3.) Change directories to the project
```console
cd casandra-ose3
```

4.) We now have to build a new image.  The Kubernetes API server was hardcoded in the Java class that's provided.  I altered the Java code to be a bit more flexible and I will discuss that later.
```console
cd image
docker build -t cassandra:latest .
```

5.) Wait a minute patiently...

6.) Save the image (or put it in a registry, your call) and copy it to all your nodes
```console
docker save -o cassandra.tgz cassandra
scp cassandra.tgz to.wherever:~/
```

7.) If you saved the images in step 6, on each node load them into Docker
```console
docker load -i cassandra.tgz
```

8.) Switch to the openshift admin.  Create a persistent volume using the persistent volume file in the project (pv-cassandra.yaml).  You probably want to edit this first to point to your NFS server.  Yes, you need an NFS server.
```console
oc create -f pv-cassandra.yaml
```

9.) Before we switch back to your user, we are going to give all service accounts the ability to access the API in the project namespace
```
oc policy add-role-to-group view system:serviceaccounts -n <<project name>>>
```

10.) Switch back to the project user

11.) Change directory back to the project directory
```console
cd casandra-ose
```

12.) Create a claim on the persistent volume
```console
oc create -f casandra-pvc.yaml
```

13.) Create the cassandra service
```console
oc create -f cassandra-service.yaml
```

13.) Earlier I mentioned I altered the Java code.  I changed it such that you now specify your Kube API server as an environment variable.  Open up the cassandra-controller.yaml and change the value to point to your OpenShift master.  Don't forget the port.
```console
vim cassandra-controller.yaml
```

14.) Create the controller.
```console
oc create -f cassandra-controller.yaml
```

15.) Execute the tests in the other readme file.
