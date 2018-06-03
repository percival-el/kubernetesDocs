---
description: Etcd in kubernetes
---

# Key Value Store \(etcd\)

Etcd is a distributed, consistent key-value store used for configuration management, service discovery, and coordinating distributed work, Etcd uses the Raft consensus algorithm to manage a highly-available replicated log.

Because it’s distributed, you can run more than one etcd instance to provide both high availability and better performance.

Etcd is designed to reliably store infrequently updated data and provide reliable watch queries. etcd exposes previous versions of key-value pairs to support inexpensive snapshots and watch history events

###  **How Kubernetes uses etcd ?**

Pods, ReplicationControllers, Services, Secrets, and so on—need to be stored somewhere in a persistent manner so their manifests survive API server restarts and failures. For this, Kubernetes uses etcd. The only component that talks to etcd directly is the Kubernetes API server. All other components read and write data to etcd indirectly through the API server.

Etcd also implements a **watch** feature, which provides an event-based interface for asynchronously monitoring changes to keys. Once a key is changed, its "watchers" get notified. This is a crucial feature in the context of Kubernetes, as the API Server component heavily relies on this to get notified and call the appropriate business logic components to move the current state towards the desired state

The desired state of a kubernetes objects created are store in etcd

* State of the cluster
* What nodes exist in the cluster
* What pods should be running, which nodes they are running on

### **Ensuring consistency when etcd is clustered**

For ensuring high availability, you’ll usually run more than a single instance of etcd. Multiple etcd instances will need to remain consistent. Such a distributed system needs to reach a consensus on what the actual state is. etcd uses the RAFT consensus algorithm to achieve this, which ensures that at any given moment, each node’s state is either what the majority of the nodes agrees is the current state or is one of the previously agreed upon states.

![Image source : kubernetes in action ](.gitbook/assets/11fig02_alt-2.jpg)

The consensus algorithm requires a majority \(or quorum\) for the cluster to progress to the next state. As a result, if the cluster splits into two disconnected groups of nodes, the state in the two groups can never diverge, because to transition from the previous state to the new one, there needs to be more than half of the nodes taking part in the state change. If one group contains the majority of all nodes, the other one obviously doesn’t. The first group can modify the cluster state, whereas the other one can’t. When the two groups reconnect, the second group can catch up with the state in the first group 

### T**he number of etcd instances should be an odd numbers**

Having two instances requires both instances to be present to have a majority. If either of them fails, the etcd cluster can’t transition to a new state because no majority exists. Having two instances is worse than having only a single instance. By having two, the chance of the whole cluster failing has increased by 100%, compared to that of a single-node cluster failing.

With three instances, one instance can fail and a majority \(of two\) still exists. With four instances, you need three nodes for a majority \(two aren’t enough\). In both three- and four-instance clusters, only a single instance may fail. But when running four instances, if one fails, a higher possibility exists of an additional instance of the three remaining instances failing \(compared to a three-node cluster with one failed node and two remaining nodes\).

