<h1 align="center">etcd operator</h1> 

![Logo](_images/logo.png)


### Overview

**etcd-Cluster:**

etcd is a distributed key value store that provides a reliable way to store data across a cluster of machines. It is a open-source and available on GitHub. etcd gracefully handles leader elections during network partitions and will tolerate machine failure, including the leader.

**etcd-Operator:**

The etcd Operator creates and maintains highly-available etcd clusters on Kubernetes, allowing engineers to easily deploy and manage etcd clusters for their applications.

Following are some features of etcd-Operator.

- Create and Destroy
- Resize
- Failover
- Rolling Upgrade
- Backup and Recovery

**etcd Architectural Flow**

Etcd-cluster gets created when we apply custom resource definition on operator. User can now create users for etcd-cluster and external application can access etcd-cluster by providing credentials.

![](_images/etcd_arch.png)


### Supported Features
High availability - Multiple instances of etcd are networked together and secured. Individual failures or networking issues are transparently handled to keep your cluster up and running.

Automated updates - Rolling out a new etcd version works like all Kubernetes rolling updates. Simply declare the desired version, and the etcd service starts a safe rolling update to the new version automatically.

Backups included - Create etcd backups and restore them through the etcd Operator.

### Common Configurations
Configure TLS - Specify static TLS certs as Kubernetes secrets.

Set Node Selector and Affinity - Spread your etcd Pods across Nodes and availability zones.

Set Resource Limits - Set the Kubernetes limit and request values for your etcd Pods.

Customize Storage - Set a custom StorageClass that you would like to use.

### Objective of tutorial

This course provides the hands-on with etcd Operator, including:

- Install etcd Operator and verify its successful installation
- Create Instance of etcd Operator
- Add Users and Enable Authentication
- Resize etcd cluster
- Work on etcd Sample Application
