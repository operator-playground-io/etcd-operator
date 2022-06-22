---
title: User Creation and Authentication 
description: How to add users and enable authentication?
---
### Add Users

**Step 1:** Connect to etcd-cluster locally using kubectl

```execute
kubectl run --rm -i --tty etcd-client --image quay.io/coreos/etcd --restart=Never  --env ClusterIP=##SSH.host##  -- /bin/sh
```

**Step 2:** Execute below command to get the Client IP:

```execute
clientIP=$(kubectl get svc -n my-etcd example-client -o .spec.clusterIP}')
echo "clientIP=$clientIP"
```
**NOTE:** We set environment variable ETCDCTL_API=3 to use v3 API(ETCDCTL_API=2 to use v2 API). If not set, it will give a warning.

**Enable Authentication**

**Step 3:** Execute the command below to add user for etcd-cluster.

```
ETCDCTL_API=3 etcdctl --endpoints http://$clientIP:2379 user add etcd-user
```
It will prompt you to set a password which you need to cnfirm later.

See the sample output below.

```
Password of openlabs:
Type password of etcd-user again for confirmation:
User etcd-user created
```

**Step 4:** Check the status of the user you created by executing the command as follows:

```
ETCDCTL_API=3 etcdctl --endpoints http://$clientIP:2379 user list
```

Sample output is given below for your reference.

```
etcd-user
```

**Step 5:** Execute the command below to exit the terminal:

```execute
exit
```
