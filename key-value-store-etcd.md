---
description: How Kubernetes uses etcd ?
---

# Key Value Store \(etcd\)

Etcd is a distributed, consistent key-value store used for configuration management, service discovery, and coordinating distributed work, Etcd uses the Raft consensus algorithm to manage a highly-available replicated log.

The desired state of a kubernetes objects created are store in etcd

Because it’s distributed, you can run more than one etcd instance to provide both high availability and better performance.

etcd is designed to reliably store infrequently updated data and provide reliable watch queries. etcd exposes previous versions of key-value pairs to support inexpensive snapshots and watch history events

Etcd also implements a **watch** feature, which provides an event-based interface for asynchronously monitoring changes to keys. Once a key is changed, its "watchers" get notified. This is a crucial feature in the context of Kubernetes, as the API Server component heavily relies on this to get notified and call the appropriate business logic components to move the current state towards the desired state

