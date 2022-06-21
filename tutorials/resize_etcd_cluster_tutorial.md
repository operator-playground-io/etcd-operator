---
title: Resize the etcd cluster
description: How to resize an etcd cluster?
---

### Resize etcd Cluster

In etcd-cluster.yaml the initial cluster size is 3.

The following procedure lets you modify the file and change the size from 3 to 5.

**Step 1:** Execute the command below to check current `etcd-cluster.yaml` and status of the Pods:

  - Check the status of cluster file

```execute
cd /home/student/code-server/etcd-operator
cat etcd-cluster.yaml
```

The sample output is given below.

```
apiVersion: etcd.database.coreos.com/v1beta2
kind: EtcdCluster
metadata:
  name: example
spec:
  size: 3
  version: 3.2.13
```

  - Check the status of Pods.

```execute
kubectl get pods -n my-etcd
```

This produces the following details.

```
NAME                             READY   STATUS    RESTARTS   AGE
etcd-operator-546468f574-7rkk5   3/3     Running   0          37m
example-h9wk7l2hp4               1/1     Running   0          35m
example-nnjcbwsdrn               1/1     Running   0          36m
example-trgkbsl8lf               1/1     Running   0          36m
```

**Note:** Please wait until the `STATUS` is `Running` and `READY` value is `1/1` or as per defined instances, and then proceed.

**Step 2:** Execute the following command to create YAML for resizing the cluster.

```execute
cat <<'EOF' > etcd-cluster.yaml
apiVersion: etcd.database.coreos.com/v1beta2
kind: EtcdCluster
metadata:
  name: example
spec:
  size: 5
  version: 3.2.13
EOF
```
**Step 3:** Execute the below command to resize the cluster.

```execute
kubectl apply -f etcd-cluster.yaml -n my-etcd
```

See the sample output below. 

```
etcdcluster.etcd.database.coreos.com/example configured
```

- Run the following command to check status of Pods.

```execute
kubectl get pods -n my-etcd
```

This should produce the below output.

```
NAME                             READY   STATUS    RESTARTS   AGE
etcd-operator-546468f574-r8mj2   3/3     Running   0          13m
example-57zlcvcrpp               1/1     Running   0          12m
example-g9rktt662f               1/1     Running   0          12m
example-j5nr8kmcgx               1/1     Running   0          11m
example-kbp7vtcsrd               1/1     Running   0          74s
example-sklwmxg5lv               1/1     Running   0          34s
```
**Note:** Please wait until the `STATUS` is `Running` and `READY` value is `1/1` or as per defined instances, and then proceed.
