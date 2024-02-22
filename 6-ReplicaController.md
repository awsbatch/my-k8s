# Kubernetes ReplicationController

### Overview on Replication Controllers
- A ReplicationController is a Kubernetes resource that ensures its pods are always kept running.
- If the pod disappears for any reason, such as in the event of a node disappearing from the cluster or because the pod was evicted from the node, the ReplicationController notices the missing pod and creates a replacement pod.
- The ReplicationController in general, are meant to create and manage multiple copies (replicas) of a pod

![image](https://github.com/awsbatch/my-k8s/assets/110165635/25f2fc65-3958-4ece-89f6-04bfcbab75dd)


- It is possible that you Node is out of resources while creating new pods with Replication controllers or replica sets, in such case it will automatically create new pods on another available cluster node

