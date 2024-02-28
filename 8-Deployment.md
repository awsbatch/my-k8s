# Relationship among Pods, ReplicaSet and Deployment

![image](https://github.com/awsbatch/my-k8s/assets/110165635/ef60bcd1-6bb8-4203-9d56-d58ccb399996)



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


# Update a Kubernetes Deployment

```
kubectl set image deploy nginx-deploy nginx-container=nginx:1.9.1
```


## Check the status of the deployment

```
kubectl rollout status deployment/nginx-deploy
```

### Rolling Update

In order to support rolling update, we need to configure the update strategy first.

So we add following part into spec

- minReadySeconds:
  - the bootup time of your application, Kubernetes waits specific time til the next pod creation.
  - Kubernetes assume that your application is available once the pod created by default.
  - If you leave this field empty, the service may be unavailable after the update process cause all the application pods are not ready yet

- maxSurge:
  - amount of pods more than the desired number of Pods
  -  this fields can be an absolute number or the percentage
  - ex. maxSurge: 1 means that there will be at most 4 pods during the update process if replicas is 3

- maxUnavailable:
  - amount of pods that can be unavailable during the update process
  - this fields can be a absolute number or the percentage
  - this fields cannot be 0 if maxSurge is set to 0
  - ex. maxUnavailable: 1 means that there will be at most 1 pod unavailable during the update process


```
apiVersion: v1
kind: Deployment
metadata:
  name: nginx-test
spec:
  replicas: 10
  selector:
    matchLabels:
      service: http-server
  strategy:
    type: RollingUpdate
    rollingUpdate:
      maxSurge: 1
      maxUnavailable: 1
  minReadySeconds: 5
  template:
    metadata:
      labels:
        service: http-server
    spec:
      containers:
      - name: nginx
        image: nginx:1.10.2
        imagePullPolicy: IfNotPresent
        ports:
        - containerPort: 80
```

### Lets apply the new nginx.yaml

```
 kubectl apply -f nginx.yaml --record
```

### Rollback Kubernetes Deployment
- #### set image

```
kubectl set image deploy nginx-deploy nginx-container=nginx:1.91 --record
```

- #### Replace

```
spec:
  containers:
  - name: nginx
    # newer image version
    image: nginx:1.11.5
    imagePullPolicy: IfNotPresent
    ports:
    - containerPort: 80
```

- ####  Edit

```
kubectl edit deployment nginx --record
```
#### Rollout Status
```
 kubectl rollout history deployment/nginx-deploy
```

#### Pause Rolling Update

```
kubectl rollout pause deployment <deployment>
```
#### Resume Rolling Update

```
kubectl rollout resume deployment <deployment>
```

### Rollback 
- After the image update, your colleague finds the service become unstable you may want to go back to the previous version. Unfortunately, he/she dunno how the previous config looks like. Well, you don’t need the time machine, just let rollback to do its job.

- At previous part, the parameter --record comes with command let the Kubernetes record the command you typed, so that you can distinguish between the revisions.

```
kubectl rollout history deployment/nginx
```


#### to previous revision
```
kubectl rollout undo deployment <deployment>
```
#### to specific revision
```
kubectl rollout undo deployment <deployment> --to-revision=<revision-number>
# exmaple
kubectl rollout undo deployment nginx --to-revision=1
```

All revision history is stored in the ReplicaSets that deployment controls. If you want to keep more revision history, please set .spec.revisionHistoryLimit in yaml to specify the number of old ReplicaSets to retain to allow rollback. (set this field at the first time apply)

```
...
spec:
  replicas: 10
  selector:
    matchLabels:
      service: http-server
  strategy:
    type: RollingUpdate
    rollingUpdate:
      maxSurge: 1
      maxUnavailable: 1
  minReadySeconds: 5
  revisionHistoryLimit: 10
...
```

```
kubectl rollout undo deployment/nginx-deploy
```

### Scale-Up of a Kubernetes Deployment

```
kubectl scale deployment nginx-deploy –replicas=5
```


