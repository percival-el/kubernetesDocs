---
description: 'Getting to know the gists of it , we will dive deep a little latter'
---

# Kubernetes Architecture

![How it all fits together](.gitbook/assets/kubernetes.png)



From a physical perspective, a Kubernetes cluster consists of:

1. A master \(with several independent sub-components, details below\) that coordinates the work.
2. A distributed key-value store, currently etcd, for maintaining the resource state in a persistent and reliable manner, throughout the cluster.
3. A number of nodes that carry out the work.

![ kubernetes Internals](.gitbook/assets/kubernetes-4.png)

