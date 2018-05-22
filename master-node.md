# Master Node

Masters acts as the primary **control plane** for kubernetes. 

Kubernetes system components communicate only with the API server. They don’t talk to each other directly. The API server is the only component that communicates with etcd. None of the other components communicate with etcd directly, but instead modify the cluster state by talking to the API server.

![](.gitbook/assets/kubernetes-34.png)

