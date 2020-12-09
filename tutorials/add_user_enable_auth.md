---
title: How to to Add Users and Enable Authentication
description: This tutorial explains how to to Add Users and Enable Authentication
---

#Connect to etcd-cluster locally using kubectl

```execute
kubectl run --rm -i --tty etcd-client --image quay.io/coreos/etcd --restart=Never  --env ClusterIP=##ssh.host##  -- /bin/sh
```

**Step 1:** Create user for etcd-cluster. 

Execute below command to add user for etcd-cluster . It will prompt for password which you need to enter and confirm.

```execute
ETCDCTL_API=3 etcdctl --endpoints http://##ssh.host##:32379 user add Openlabs
```

Sample output:

```
Password of openlabs:
Type password of openlabs again for confirmation:
User openlabs created
```

**Step 2:** Checking the user creating is successful or not

Execute below command to check created usuer:

```execute
ETCDCTL_API=3 etcdctl --endpoints http://##ssh.host##:32379 user list
```

Sample output:

```
openlabs
```

Execute below command to exit out of terminal:

```execute
exit
```
