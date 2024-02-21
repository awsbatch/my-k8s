![image](https://github.com/awsbatch/my-k8s/assets/110165635/a35ee506-7092-44d3-af0a-b37a014bfab5)




#  <div align="center">Kubernetes Core Concepts -Pods</div>



##  What is a Kubernetes Pod?

A Kubernetes Pod is the smallest, most basic deployable object in Kubernetes. A Pod represents a single instance of a running process in your cluster. Pods contain one or more containers, such as Docker containers. When a Pod runs multiple containers, the containers are managed as a single entity and share the Pod's resources, such as networking and storage.

As per the official Kubernetes documentation Pods are the smallest deployable units of computing that you can create and manage in Kubernetes.
A pod encapsulates applications running in one or more Linux/Windows containers.
### All the containers within a single pod shares following things:

- A unique network IP
- Network Namespace
- Storage volumes
- All other specifications applied to the pod

# Why we need pods when we have containers?
It solves port allocation issue. With containers we can use a specific host port only once. Since a Pod works like a standalone virtual host having a unique IP the same port can be used multiple times without encountering conflicts.

# What is the job of a Pod?
- At a very high level a Pod is responsible to carry your application running inside within a container.

# How does a pod work?
- Pods are group of containers working together to process a set of desired work. When we create a Pod either using an YAML manifest file or by the kubectl command we specify the desired behavior a pod should acquit itself.

- Kubernetes Controllers such as StatefulSets, Deployments and DaemonSet are responsible for creating workloads using Pods. They manage Pods throughout their lifecycle for example while rolling out updates, scaling up/down ReplicaSets and managing their health within the cluster.

- We will learn about these controllers in detail in our upcoming classes.

# Lifecycle of a Pod
- Pods follow a defined lifecycle, starts with the Pending state, moving through Running if at least one of its containers starts OK, and then through either the Succeeded or Failed phases depending on whether any container in the Pod terminated in failure state.


![image](https://github.com/awsbatch/my-k8s/assets/110165635/b4eb6bc4-e907-4ecf-8a38-f70ca5dc025d)


#### Here are the possible values for phase:

![image](https://github.com/awsbatch/my-k8s/assets/110165635/ece80ca5-92ca-4ce1-b6d9-4ebbf5eb4709)



# Pod Lifetime

- Kubernetes pods are transient in nature, if a pod (or the node it executes on) fails, Kubernetes automatically creates a new instance of that pod on the node they were scheduled to run until they die or node itself goes down in order to continue operations. Each Pod will have a unique id assigned (UID) to it.

- A running Pod can never be "rescheduled" to a different node as Kubernetes does not support live migration of pods from one host to another. You can always create a new identical pod.

- Pods by nature doesn't self-heal. This is controlled by another Kubernetes components known as Controllers. They manage the Pods throughout their lifecycle.



# Create a Kubernetes Pod
#### There are two methods of creating a Kubernetes Pods.

# Method 1 - using kubectl command
#### Kubectl command controls the Kubernetes Cluster.

```
kubectl run lco-pod-demo --image=nginx
```

### Verify the status of Pod created -

```
kubectl get pods
```
### Describe the Pod created in last step to get more details about it -

```
kubectl describe pod lco-pod-demo
```

#### Few fields which requires your attention here are -

- Status
- IP
- Labels
- Container specs
- Events


# Method 2 - using yaml manifest file


Here is our yaml manifest file which we are going to use to create a Pod.

```
apiVersion: v1
kind: Pod
metadata:
   name: lco-pod-demo-2
   namespace: default
   labels:
     demo: pod
spec:
  containers:
  - name: httpd
    image: docker.io/httpd
    imagePullPolicy: IfNotPresent
    ports:
      - containerPort: 80
```



