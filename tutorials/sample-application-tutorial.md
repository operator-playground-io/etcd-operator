---
title: Use Cluster Resource in a Sample Application 
description: How to use an existing etcd cluster in an application? 
---

### Introduction

In this tutorial, we take an example of Shopping List application, which is a Node.js application deployed as a microservice.
The example also uses Skaffold which handles the workflow for building, pushing and deploying your application, allowing you to focus on writing code.

### Code Structure

![codestructure](_images/shopping-app-structure.png)

The code structure follows a simple modular and MVC pattern. There are 2 folders that are of our interest:
- k8s :  This folder contains all the deployment and service yaml for the application. This defines the deployment and exposure of our application.
- backend: This folder contains all the backend code made using Node.js (Express). The frontend is built using EJS JavaScript template framework.

### Try the example

**Step 1:** Create an etcd cluster executing these commands. 

If you have already installed the etcd operator and have followed the steps to create an etcd cluster you can skip this step.

```execute
cat <<'EOF' >etcd-cluster.yaml
apiVersion: etcd.database.coreos.com/v1beta2
kind: EtcdCluster
metadata:
  name: example
spec:
  size: 1
  version: 3.2.13
EOF
```

```execute
kubectl create -f etcd-cluster.yaml -n my-etcd
```

See below for the sample output:

```
etcdcluster.etcd.database.coreos.com/example created
```

```execute
kubectl get pods -n my-etcd
```

This produces an output like the below:

```
NAME                             READY   STATUS     RESTARTS   AGE
etcd-operator-546468f574-78vxg   3/3     Running    0          11m
example-xdsgpp9c6s               0/1     Init:0/1   0          24s
```

It may take up to a few minutes until all the resources are created and the cluster is ready for use.

```
NAME                             READY   STATUS    RESTARTS   AGE
etcd-operator-546468f574-78vxg   3/3     Running   0          30m
example-xdsgpp9c6s               1/1     Running   0           1m
```

**Note:** Please wait until the `STATUS` is `Running` and `READY` value is `1/1` or as per defined instances, and then proceed.

**Step 2:** Install the application sample

- Get sample code.

```execute
cd /home/student/projects && git clone https://github.com/Andi-Cirjaliu/edge-node-etcd-shopping-deploy.git
```

- Navigate to the example.
```execute
cd /home/student/projects/edge-node-etcd-shopping-deploy
```

- Setup skaffold default repository to the local one.
```execute
skaffold config set default-repo localhost:5000
```

- Install and start the sample.

```execute
skaffold run
```
Alternatively you can use this command to install the sample, watch for code changes and re-deploy the application automatically. On exiting the command, Skaffold will automatically stop and delete the sample application. 

```execute
skaffold dev
```

To stop and remove the application, refer the step-by-step guide in Clean up the Kubernetes resources section.

### Access the application

-	Click on the Key icon on the dashboard and copy the value under the `DNS` section and `IP` field.
Follow the URL: http://##DNS.ip##:30100 


### Deploy changes to Kubernetes in Dev Mode

In this example , we use `Skaffold` which simplifies local development. You can deploy the application is DEV mode which keeps watching for the files changes and on any change, triggers the entire deployment process automatically without the user having to run and manage it manually.

- Navigate to the example:

```execute
cd /home/student/projects/edge-node-etcd-shopping-deploy
```

```execute
skaffold dev
```

On exiting the command, Skaffold will automatically destroy all the resources it created with above command.


Also, you can use the `skaffold run` to deploy the changes onto Kubernetes as a normal mode. In this mode, the resources created remains unless the user deletes them.

### Clean up the Kubernetes resources

You can delete all the application resources created by executing the following command:

```execute
skaffold delete
```
