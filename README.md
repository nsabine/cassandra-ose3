##Making This Example Work in OSE3

1.) Clone the repository into an OSE3 user's working directory
```console
git@github.com:kevensen/cassandra-ose3.git
```

2.) Login to OSE3
```console
oc login
```

3.) Switch to the openshift admin.  Create a persistent volume using the persistent volume file in the project (pv-cassandra.yaml).  You probably want to edit this first to point to your NFS server.  Yes, you need an NFS server.
```console
oc create -f pv-cassandra.yaml
```

4.) Before we switch back to your user, we are going to give all service accounts the ability to access the API in the project namespace
```
oc policy add-role-to-group view system:serviceaccounts -n <<project name>>>
```

5.) Switch back to the project user

6.) Change directory back to the project directory
```console
cd casandra-ose
```

7.) Create a claim on the persistent volume
```console
oc create -f casandra-pvc.yaml
```

8.) Create the cassandra service
```console
oc create -f cassandra-service.yaml
```

9.) Earlier I mentioned I altered the Java code.  I changed it such that you now specify your Kube API server as an environment variable.  Open up the cassandra-controller.yaml and change the value to point to your OpenShift master.  Don't forget the port.
```console
vim cassandra-controller.yaml
```

10.) Create the controller.
```console
oc create -f cassandra-controller.yaml
```

11.) Execute the tests in the other readme file.
