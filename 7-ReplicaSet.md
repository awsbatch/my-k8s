# ReplicaSets


![image](https://github.com/awsbatch/my-k8s/assets/110165635/7d20d66c-8c38-4eb4-a49d-9ed9684c849c)


# What is ReplicaSet ?

#### - ReplicaSet object is used to maintain a stable set of replicated pods running within a cluster at any given time. Its purpose is to maintain the specified number of pods and prevent the user from loosing the control of the application in case of any pod failure or inaccessible. Whenever any pod gets failed replicaset immediately launches another pod and replacing it with failed pod.

# Comparing a ReplicaSet to a ReplicationController

- A ReplicaSet behaves exactly like a ReplicationController, but it has more expressive pod selectors.

- Whereas a ReplicationController’s label selector only allows matching pods that include a certain label, a ReplicaSet’s selector also allows matching pods that lack a certain label or pods that include a certain label key, regardless of its value.

- Also, for example, a single ReplicationController can’t match pods with the label env=production and those with the label env=devel at the same time. It can only match either pods with the env=production label or pods with the env=devel label. But a single ReplicaSet can match both sets of pods and treat them as a single group.

- Similarly, a ReplicationController can’t match pods based merely on the presence of a label key, regardless of its value, whereas a 
ReplicaSet can. For example, a ReplicaSet can match all pods that include a label with the key env, whatever its actual value is (you can think of it as env=*).


# Example-1: Create replica set using match labels


#### We will create a new replica set which will now map the orphaned pods from replication controller. But before that we need to have the KIND and apiVersion value for replica set.

```
[root@controller ~]# kubectl api-resources | grep -iE 'KIND|replica'
NAME                              SHORTNAMES   APIGROUP                       NAMESPACED   KIND
replicationcontrollers            rc                                          true         ReplicationController
replicasets                       rs           apps                           true         ReplicaSet
```


```
[root@controller ~]# kubectl explain ReplicaSet | head -n 2
KIND:     ReplicaSet
VERSION:  apps/v1
```



```
[root@controller ~]# cat replica-set.yml
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: myapp-replicaset
  labels:
    app: myapp
    type: dev
spec:
  replicas: 3
  selector:
    matchLabels:
      app: myapp
  template:
    metadata:
      name: myapp-pod
      labels:
        app: myapp
        type: dev
    spec:
      containers:
      - name: nginx-container
        image: nginx
```

```
[root@controller ~]# kubectl apply -f replica-set.yml
replicaset.apps/myapp-replicaset created
```

```
[root@controller ~]# kubectl get pods -o wide
NAME                        READY   STATUS    RESTARTS   AGE    IP          NODE                   NOMINATED NODE   READINESS GATES
myapp-rc-6vjv4              1/1     Running   1          12h    10.36.0.4   worker-1.example.com   <none>           <none>
myapp-rc-9fp5l              1/1     Running   1          12h    10.36.0.3   worker-1.example.com   <none>           <none>
myapp-rc-cwwwh              1/1     Running   1          12h    10.44.0.4   worker-2.example.com   <none>           <none>
```

```
[root@controller ~]# kubectl describe pods myapp-rc-6vjv4
Name:         myapp-rc-6vjv4
Namespace:    default
Priority:     0
Node:         worker-1.example.com/192.168.43.49
Start Time:   Mon, 30 Nov 2020 00:23:57 +0530
Labels:       app=myapp
              type=dev
Annotations:  <none>
Status:       Running
IP:           10.36.0.4
IPs:
  IP:           10.36.0.4
Controlled By:  ReplicaSet/myapp-replicaset
...
```

```
[root@controller ~]# kubectl get rs
NAME               DESIRED   CURRENT   READY   AGE
myapp-replicaset   3         3         3       46s
```

```
[root@controller ~]# kubectl describe rs myapp-replicaset
Name:         myapp-replicaset
Namespace:    default
Selector:     app=myapp
Labels:       app=myapp
              type=dev
Annotations:  <none>
Replicas:     3 current / 3 desired
Pods Status:  3 Running / 0 Waiting / 0 Succeeded / 0 Failed
Pod Template:
  Labels:  app=myapp
           type=dev
  Containers:
   nginx-container:
    Image:        nginx
    Port:         <none>
    Host Port:    <none>
    Environment:  <none>
    Mounts:       <none>
  Volumes:        <none>
Events:           <none>
```

# Example-2: Create replica set using match expressions

- #### The main improvements of ReplicaSets over ReplicationControllers are their more expressive label selectors. You intentionally used the simpler matchLabels selector in the first ReplicaSet example to see that ReplicaSets are no different from Replication-Controllers.

- #### Now, we will rewrite the selector to use the more powerful matchExpressions property:

```
[root@controller ~]# cat replica-set.yml
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: myapp-replicaset
  labels:
    app: myapp
    type: dev
spec:
  replicas: 3
  selector:
    matchExpressions:
    - key: app
      operator: In
      values:
      - myapp
  template:
    metadata:
      name: myapp-pod
      labels:
        app: myapp
        type: dev
    spec:
      containers:
      - name: nginx-container
        image: nginx
```

```
---
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: httpd-replica
  namespace: facebook
spec:
  replicas: 4
  minReadySeconds: 10
  selector:
    matchLabels:
      role: web
    matchExpressions:
      - {key: version, operator: In, values: [v1, v2, v3]}
  template:
    metadata:
      name: web
      labels:
        role: web
        version: v1
        tier: frond
    spec:
      containers:
      - name: web
        image: httpd
        ports:
        - containerPort: 80
          protocol: TCP
```


Here, this selector requires the pod to contain a label with the “app” key and the label’s value must be “myapp”.

You can add additional expressions to the selector. As in the example, each expression must contain a key, an operator, and possibly (depending on the operator) a list of values. You’ll see four valid operators:

In: Label’s value must match one of the specified values.
NotIn: Label’s value must not match any of the specified values.
Exists: Pod must include a label with the specified key (the value isn’t important). When using this operator, you shouldn’t specify the values field.
DoesNotExist: Pod must not include a label with the specified key. The values property must not be specified.

```
[root@controller ~]# kubectl apply -f replica-set.yml
replicaset.apps/myapp-replicaset created
```

```
[root@controller ~]# kubectl delete rs myapp-replicaset
replicaset.apps "myapp-replicaset" deleted
```

## Horizontally scaling Pod

```
[root@controller ~]# kubectl scale rs myapp-replicaset --replicas=6
replicaset.apps/myapp-replicaset scaled
```

```
[root@controller ~]# kubectl get pods -o wide
NAME                        READY   STATUS              RESTARTS   AGE    IP          NODE                   NOMINATED NODE   READINESS GATES
myapp-rc-6vjv4              1/1     Running             1          12h    10.36.0.4   worker-1.example.com   <none>           <none>
myapp-rc-9fp5l              1/1     Running             1          12h    10.36.0.3   worker-1.example.com   <none>           <none>
myapp-rc-cwwwh              1/1     Running             1          12h    10.44.0.4   worker-2.example.com   <none>           <none>
myapp-replicaset-8r6kx      0/1     ContainerCreating   0          6s     <none>      worker-2.example.com   <none>           <none>
myapp-replicaset-kp78z      0/1     ContainerCreating   0          6s     <none>      worker-1.example.com   <none>           <none>
myapp-replicaset-svm45      0/1     ContainerCreating   0          6s     <none>      worker-1.example.com   <none>           <none>
```

```
[root@controller ~]# kubectl scale rs myapp-replicaset --replicas=3
replicaset.apps/myapp-replicaset scaled
```

```
[root@controller ~]# kubectl get pods
NAME                        READY   STATUS        RESTARTS   AGE
myapp-rc-6vjv4              1/1     Running       1          12h
myapp-rc-9fp5l              1/1     Running       1          12h
myapp-rc-cwwwh              1/1     Running       1          12h
myapp-replicaset-8r6kx      0/1     Terminating   0          11m
myapp-replicaset-kp78z      0/1     Terminating   0          11m
myapp-replicaset-svm45      0/1     Terminating   0          11m
```

