# Kubernetes ReplicationController

### Overview on Replication Controllers
- A ReplicationController is a Kubernetes resource that ensures its pods are always kept running.
- If the pod disappears for any reason, such as in the event of a node disappearing from the cluster or because the pod was evicted from the node, the ReplicationController notices the missing pod and creates a replacement pod.
- The ReplicationController in general, are meant to create and manage multiple copies (replicas) of a pod
