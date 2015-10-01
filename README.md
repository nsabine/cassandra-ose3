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

7.) Create the Cassandra cluster
```console
oc create -f cassandra.yaml
```

8.) Verify three Pods are in Running state
```console
oc get pods -l name=cassandra
```

9.) Verify three Pods are in Running state
```console
oc get pods -l name=cassandra
```

10.) Check the pod logs as cassandra is starting.  Replace POD_ID with the name of the cassandra pod from `oc get pods`.
```console
oc logs POD_ID 
```

11.) Verify Cassandra cluster is running with all three cluster members.  Replace POD_ID with the name of the cassandra pod from `oc get pods`.
```console
oc exec POD_ID -- nodetool status
```
