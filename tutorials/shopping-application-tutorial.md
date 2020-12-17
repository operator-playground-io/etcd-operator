```
---
title: etcd Operator Sample Application Tutorial
description: This tutorial explains how to use an etcd cluster created by the operator in an application.
---

### Introduction

Shopping List application is a Node.js application which is deployed as a microservice.
The example also uses Skaffold which handles the workflow for building, pushing and deploying your application, allowing you to focus on what matters most: writing code.

### Access the application

Click on the Key icon on the dashboard and copy the value under the `DNS` section and `IP` field

URL :  http://##DNS.ip##:30456

### Code Structure

![codestructure](_images/shopping-app-structure.png)

It follows a simple modular and MVC pattern. There are 2 folders that are of our interest:
- k8s :  This contains all the deployment and service yaml for the application. This defines the deployment and exposure of our application.
- backend: This contains all the backend code made using Node.js (Express). The frontend is built using EJS JavaScript template framework.

### Try the example

**step 1:** Create an etcd cluster executing these commands. If you already installed the etcd operator and followed the steps to create an etcd cluster you can skip this step.

```execute
kubectl create -f etcd-cluster.yaml -n my-etcd
```

Sample output:

```
etcdcluster.etcd.database.coreos.com/example created
```

```execute
kubectl get pods -n my-etcd
```

You will see output similar like below:

```
NAME                             READY   STATUS     RESTARTS   AGE
etcd-operator-546468f574-78vxg   3/3     Running    0          11m
example-7m2dq8mk5d               1/1     Running    0          45s
example-xdsgpp9c6s               0/1     Init:0/1   0          24s
```

It may take up to a few minutes until all the resources are created and the cluster is ready for use.

```
NAME                             READY   STATUS    RESTARTS   AGE
etcd-operator-546468f574-78vxg   3/3     Running   0          30m
example-7m2dq8mk5d               1/1     Running   0          20m
example-vfbs98thmq               1/1     Running   0          19m
example-xdsgpp9c6s               1/1     Running   0          19m
```

**Note - Please wait till `Status` will be `Running` and `READY` should be 1/1 , and then proceed further.**

**step 2:** Install the application sample

Get sample code
```execute
cd /home/student/projects && git clone https://github.com/Andi-Cirjaliu/edge-node-etcd-shopping-deploy.git
```

Setup skaffold
```execute
cd /home/student/projects/edge-node-etcd-shopping-deploy && skaffold config set default-repo localhost:5000
```

Install the sample
```execute
skaffold run
```
Alternatively you can use this command to install the sample, watch for code changes and re-deploy the application automatically
```execute
skaffold dev
```

### Deploy changes to Kubernetes in Dev Mode

Go to Developer Dashboard tab, it will provide you with the IDE along with the integrated terminal.  Click on the bottom status bar and select `TERMINAL`. 

k8s folder contains all the manifest files and defines the deployment stategy for the application.
One can execute them using :

```execute
kubectl apply -f k8s/
```

In this example , we use `Skaffold` which simplifies local devlopment. You can deploy the application is DEV mode which keeps watching for the files changes and on any change, triggers the entire deployment process automatically without the user having to run and manage it manually.

Navigate to the example:

```execute
cd /home/student/projects/edge-node-etcd-shopping-deploy
```

```execute
skaffold dev --default-repo localhost:5000/app
```

On exiting the command, Skaffold will automatically destroy all the resources it created with above command.


Also, you can use the `skaffold run --default-repo localhost:5000/app` to deploy the changes onto kubernetes as a normal mode. In this mode, the resources created remains unless the user deletes them.

### Clean up the Kubernetes resources

You can delete all the application resources created by executing the following command:

```execute
skaffold delete
```
