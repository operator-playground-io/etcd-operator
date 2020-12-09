---
title: etcd Operator Tutorial
description: This tutorial explains basics about etcd Operator
---

### Introduction

**etcd-Cluster:**

etcd is a distributed key value store that provides a reliable way to store data across a cluster of machines. It is a open-source and available on GitHub. etcd gracefully handles leader elections during network partitions and will tolerate machine failure, including the leader.

**etcd-Operator:**

The etcd Operator creates and maintains highly-available etcd clusters on Kubernetes, allowing engineers to easily deploy and manage etcd clusters for their applications.

- Create and Destroy
- Resize
- Failover
- Rolling Upgrade
- Backup and Recovery

**etcd Architectural Flow**

To create etcd-cluster instance user need to install olm and deploy etcd operator on kubernetes. etcd-cluster gets created when we apply custom resource definition on operator. User can now create users for etcd-cluster and external application can access etcd-cluster by providing credentials.

![](_images/etcd_arch.png)

### Check the etcd Operator creation status 

Execute the following command to check if the etcd Operator is running:
```execute
kubectl get csv -n my-etcd | egrep -i "name|etcd"
```

Sample output:

```
NAME                             DISPLAY                       VERSION       REPLACES                   PHASE
elastic-cloud-eck.v1.0.0-beta1   Elastic Cloud on Kubernetes   1.0.0-beta1   elastic-cloud-eck.v0.9.0   Succeeded
etcdoperator.v0.9.4              etcd                          0.9.4         etcdoperator.v0.9.2        Succeeded

```

**Note - Please wait till `PHASE` will be `Succeeded` and then proceed further.**

