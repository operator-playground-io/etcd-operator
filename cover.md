<h1 align="center">etcd operator</h1> 

![Logo](_images/logo.png)


### Overview

This section introduces the following fundamental terminologies.

**1. etcd-Cluster:**

etcd is a consistent, distributed and highly available key value store that provides an efficient and reliable way to store data across a cluster of machines within Kubernetes environment. If you wish to user etcd as a backing store for your Kubernetes cluster, verify that you have a backup plan ready for the intended data. etcd gracefully handles leader elections during network partitions and is able to tolerate machine failures, including the leader.

**2. etcd-Operator:**

The etcd Operator creates and maintains highly-available etcd clusters on Kubernetes, allowing engineers to easily deploy and manage etcd clusters for their applications.

Following are a few striking features of etcd-Operator.

- Create and Destroy
- Resize
- Failover
- Rolling Upgrade
- Backup and Recovery

### Architecture of etcd Operator

Etcd-cluster gets created when you apply custom resource definition on the operator. User can now create more users for etcd-cluster and external application can access etcd-cluster by leveraging the set credentials.

![](_images/etcd_arch.png)

### Common Configurations

- Configure TLS - Specify static TLS certs as Kubernetes secrets.
- Set Node Selector and Affinity - Spread your etcd Pods across Nodes and availability zones.
- Set Resource Limits - Set the Kubernetes limit and request values for your etcd Pods.
- Customize Storage - Set a custom StorageClass that you would like to use.


### Supported Features
1. High availability - Multiple instances of etcd are networked together and secured. Individual failures or networking issues are transparently handled to keep your cluster up and running.

2. Automated updates - Rolling out a new etcd version works like all Kubernetes rolling updates. Simply declare the desired version, and the etcd service starts a safe rolling update to the new version automatically.

3. Inclusive Backups - Create etcd backups and restore them through the etcd Operator.

### Objective of the tutorial

This tutorial is designed to provide you hands-on training on provides the hands-on with etcd Operator, that includes the following topics:

- Install etcd Operator and verify its successful installation
- Create an instance of etcd Operator
- Add users and enable authentication
- Resize etcd cluster
- Work on etcd Sample Application
