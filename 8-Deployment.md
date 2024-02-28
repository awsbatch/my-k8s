
![image](https://github.com/awsbatch/my-k8s/assets/110165635/7ef389d9-e79f-4f81-a858-9fc7c1a5cb94)


# Kubernetes Deployment

- When running Pods in datacenter, additional features may be needed such as scalability, updates and rollback etc which are offered by Deployments
- A Deployment is a higher-level resource meant for deploying applications and updating them declaratively, instead of doing it through a - -- ReplicationController or a ReplicaSet, which are both considered lower-level concepts.
- When you create a Deployment, a ReplicaSet resource is created underneath. Replica-Sets replicate and manage pods, as well.
- When using a Deployment, the actual pods are created and managed by the Deployment’s ReplicaSets, not by the Deployment directly


![image](https://github.com/awsbatch/my-k8s/assets/110165635/6df6902b-7d01-42e6-8a9f-7bf4f84e5302)


## Create Kubernetes Deployment resource

### In the deployment spec, following properties are managed:

- replicas: explains how many copies of each Pod should be running
- strategy: explains how Pods should be updated
- selector: uses matchLabels to identify how labels are matched against the Pod
- template: contains the pod specification and is used in a deployment to create Pods


```
kubectl create deployment nginx-deploy --image=nginx --dry-run=client -o yaml > nginx-deploy.yml
```

```
[root@controller ~]# cat nginx-deploy.yml
apiVersion: apps/v1
kind: Deployment
metadata:
  creationTimestamp: null
  labels:
    app: nginx-deploy
  name: nginx-deploy
spec:
  replicas: 1
  selector:
    matchLabels:
      app: nginx-deploy
  strategy: {}
  template:
    metadata:
      creationTimestamp: null
      labels:
        app: nginx-deploy
    spec:
      containers:
      - image: nginx
        name: nginx
        resources: {}
status: {}
```
