# Kubernetes ReplicationController

### Overview on Replication Controllers
- A ReplicationController is a Kubernetes resource that ensures its pods are always kept running.
- If the pod disappears for any reason, such as in the event of a node disappearing from the cluster or because the pod was evicted from the node, the ReplicationController notices the missing pod and creates a replacement pod.
- The ReplicationController in general, are meant to create and manage multiple copies (replicas) of a pod

![image](https://github.com/awsbatch/my-k8s/assets/110165635/25f2fc65-3958-4ece-89f6-04bfcbab75dd)


- It is possible that you Node is out of resources while creating new pods with Replication controllers or replica sets, in such case it will automatically create new pods on another available cluster node

![image](https://github.com/awsbatch/my-k8s/assets/110165635/0496f776-c836-41e8-9aae-99183f369035)


# Components of RC

A replicationController consists of:

- A label selector, which determines what pods are in the ReplicationController’s scope
- A replica count, which specifies the desired number of pods that should be running
- A pod template, which is used when creating new pod replicas


# Creating a replication controller

```
[root@controller ~]# kubectl api-resources | grep -iE 'KIND|replication'
NAME                              SHORTNAMES   APIGROUP                       NAMESPACED   KIND
replicationcontrollers            rc                                          true         ReplicationController
```

So, the kind value would be ReplicationController, now to get the apiVersion of this kind we will use kubectl explain command:

```
kubectl explain ReplicationController | head -n 2
```
Now we have the kind and apiVersion value needed to create our first replication controller. Similar to pods and other Kubernetes resources, you create a ReplicationController by posting a JSON or YAML descriptor to the Kubernetes API server.

```
# cat replication-controller.yml
apiVersion: v1
kind: ReplicationController
metadata:
  name: myapp-rc
  labels:
    app: myapp
    type: dev
spec:
  replicas: 3
  selector:
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

### To create the ReplicationController, use the kubectl create command:

```
 kubectl create -f replication-controller.yml
```
### Verify the operation of Replication Controller

```
kubectl get rs
```

```
kubectl describe rc myapp-rc
```


# Changing the pod template
- A ReplicationController’s pod template can be modified at any time. Changing the pod template will only affect the newly created pods and will have no impact on the existing pods which are in running state:

- As an exercise I will update the replicas value by editing the pod template, and the change the value of replicas to 4 and save the template. This will open the ReplicationController’s YAML definition in your default text editor:

```
 kubectl edit rc myapp-rc
```


# Horizontally scaling pods

- You’ve seen how ReplicationControllers make sure a specific number of pod instances is always running. Because it’s incredibly simple to change the desired number of replicas, this also means scaling pods horizontally is trivial.

- Assuming you suddenly expect that load on your application is going to increase so you must deploy more pods until the load is reduced, in such case you can easily scale up the number of pods runtime.

- For example here I am scaling up the number of replicas to 6:

  ```
  kubectl scale rc myapp-rc --replicas=6
  ```

  ####  Similarly once the load is reduced, the replicas can be scaled down as well, here I have now reduced the number of replicas to 3. All this command does is modify the spec.replicas field of the ReplicationController’s definition—like when you changed it through kubectl edit.:

  ```
  kubectl scale rc myapp-rc --replicas=3
  ```

  ## Deleting a ReplicationController

- When you delete a ReplicationController through kubectl delete, the pods are also deleted. But because pods created by a ReplicationController aren’t an integral part of the ReplicationController, and are only managed by it, you can delete only the ReplicationController and leave the pods running.

- When deleting a ReplicationController with kubectl delete, you can keep its pods running by passing the --cascade=false option to the command.

```
kubectl delete rc myapp-rc --cascade=false
```

```
kubectl delete rc myapp-rc
```
