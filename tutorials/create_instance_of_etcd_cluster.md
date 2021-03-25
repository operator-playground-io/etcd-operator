---
title: Create Instance of etcd Cluster
description: How to create an instance of etcd cluster?
---

### Create Instance of etcd Operator

etcd-Operator Instance can be created using the Custom Resource Definition YAML files.

**Step 1:** Create a custom resource definition YAML file with given specification.

```execute
cat <<'EOF' >etcd-cluster.yaml
apiVersion: etcd.database.coreos.com/v1beta2
kind: EtcdCluster
metadata:
  name: example
spec:
  size: 3
  version: 3.2.13
EOF
```

**Step 2:** Create etcd-cluster in the same namespace of operator using kubectl command.

```execute
kubectl create -f etcd-cluster.yaml -n my-etcd
```

Sample output is given below for your reference.

```
etcdcluster.etcd.database.coreos.com/example created
```

**Step 3:** Check the status of etcd-cluster Pods.

```execute
kubectl get pods -n my-etcd
```

You will see an output like below:

```
NAME                             READY   STATUS     RESTARTS   AGE
etcd-operator-546468f574-78vxg   3/3     Running    0          11m
example-7m2dq8mk5d               1/1     Running    0          45s
example-xdsgpp9c6s               0/1     Init:0/1   0          24s
```

It may take up to a few minutes for the resources to be created and cluster to be available for use.

```
NAME                             READY   STATUS    RESTARTS   AGE
etcd-operator-546468f574-78vxg   3/3     Running   0          30m
example-7m2dq8mk5d               1/1     Running   0          20m
example-vfbs98thmq               1/1     Running   0          19m
example-xdsgpp9c6s               1/1     Running   0          19m
```

**Note:** Please wait until the `STATUS` is `Running` and `READY` value is `1/1` or as per defined instances, and then proceed.

### Access etcd-cluster from within a Pod/container

**Step 1:** Open the cluster-service

```execute
kubectl get svc -n my-etcd
```

You will see output like the below:

```
NAME                    TYPE        CLUSTER-IP       EXTERNAL-IP   PORT(S)             AGE
etcd-restore-operator   ClusterIP   10.96.179.156    <none>        19999/TCP           13m
example                 ClusterIP   None             <none>        2379/TCP,2380/TCP   2m38s
example-client          ClusterIP   10.107.108.163   <none>        2379/TCP            2m38s
```

To access etcd cluster externally, first update the service to use NodePort:

**Step 2:** Execute below command to use NodePort:

```execute
kubectl get service example-client --output yaml -n my-etcd > /tmp/my-etcd.yaml
sed -i "s/type: .*/type: NodePort/g" /tmp/my-etcd.yaml
kubectl patch svc example-client -p "$(cat /tmp/my-etcd.yaml)" --namespace my-etcd
```

**Step 3:** Execute below command to update NodePort to 32379:

```execute
kubectl get service example-client --output yaml -n my-etcd > /tmp/my-etcd.yaml
sed -i "s/nodePort: .*/nodePort: 32379/g" /tmp/my-etcd.yaml
kubectl patch svc example-client -p "$(cat /tmp/my-etcd.yaml)" --namespace my-etcd
```


**Step 4:** Connect to etcd-cluster from etcd-client container

```execute
kubectl run --rm -i --tty etcd-client --image quay.io/coreos/etcd --restart=Never  --env ClusterIP=##DNS.ip##  -- /bin/sh
```

You should see the output like below.

```
If you don't see a command prompt, try pressing enter.
/ #
```

**Step 5:** Put sample key-value pair into etcd-cluster database

```execute
ETCDCTL_API=3 etcdctl --endpoints http://##DNS.ip##:32379 put cluster admin
```

**NOTE:** We set environment variable ETCDCTL_API=3 to use v3 API(ETCDCTL_API=2 to use v2 API). If not set will give a warning.

Sample output is given below:

```
OK
```

**Step 6:** Retrieving the value by key by running the command as follows.

```execute
ETCDCTL_API=3 etcdctl --endpoints http://##DNS.ip##:32379 get cluster 
```

This should produce an output like:

```
cluster
admin
```

**Step 7:** Execute below command to exit out of terminal:

```execute
exit
```
