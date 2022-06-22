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
cd /home/student/code-server && git clone https://github.com/operator-playground-io/etcd-sample.git
```

- Navigate to the example.
```execute
cd /home/student/code-server/etcd-sample
```

- Setup skaffold default repository to the local one.
```execute
sudo /usr/local/bin/skaffold config set default-repo localhost:5000
```

- Install and start the sample.

```execute
sudo /usr/local/bin/skaffold run
```

### Access the application

-	Follow the URL: http://##SSH.host##:30100 

### Clean up the Kubernetes resources

You can delete all the application resources created by executing the following command:

```execute
sudo /usr/local/bin/skaffold delete
```
