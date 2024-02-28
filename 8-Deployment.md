
![image](https://github.com/awsbatch/my-k8s/assets/110165635/7ef389d9-e79f-4f81-a858-9fc7c1a5cb94)


# Kubernetes Deployment

- When running Pods in datacenter, additional features may be needed such as scalability, updates and rollback etc which are offered by Deployments
- A Deployment is a higher-level resource meant for deploying applications and updating them declaratively, instead of doing it through a - -- ReplicationController or a ReplicaSet, which are both considered lower-level concepts.
- When you create a Deployment, a ReplicaSet resource is created underneath. Replica-Sets replicate and manage pods, as well.
- When using a Deployment, the actual pods are created and managed by the Deployment’s ReplicaSets, not by the Deployment directly
