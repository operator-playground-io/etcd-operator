---
title: Operator Deployment Status Verification
description: How to check the configuration status of etcd Operator?
---

### Check the etcd Operator Configuration Status 

- Execute the following command to check if the etcd Operator is up and running:

```execute
kubectl get csv -n my-etcd | egrep -i "name|etcd"
```

You should see the below output.

```
NAME                             DISPLAY                       VERSION       REPLACES                   PHASE
elastic-cloud-eck.v1.0.0-beta1   Elastic Cloud on Kubernetes   1.0.0-beta1   elastic-cloud-eck.v0.9.0   Succeeded
etcdoperator.v0.9.4              etcd                          0.9.4         etcdoperator.v0.9.2        Succeeded

```

It takes up to a few minutes for the PHASE status to turn “Succeeded”. Please wait for the time it shows up, before proceeding further.

