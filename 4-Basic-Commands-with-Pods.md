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

# The manifest file you've shown defines a Kubernetes Pod resource. Let's break down its fields to understand what each one means:

- **apiVersion:** v1: This specifies the version of the Kubernetes API you're using to create this object. For a Pod, the API version is v1.

- **kind:** Pod: This indicates the kind of Kubernetes object you want to create. In this case, it's a Pod, which is the smallest deployable unit in Kubernetes that can be created and managed.

- **metadata:** This section provides data that uniquely identifies the Pod, including its name, namespace, and labels:
  - **name:** lco-pod-demo-2: This is the name of the Pod. It's unique within the namespace.
  - **namespace:** default: This specifies the namespace in which the Pod will be created. If not specified, it defaults to the default namespace.
  - **labels:** Key-value pairs that are attached to the Pod. These can be used to organize and to select subsets of objects.
    - **demo: pod:** A label with the key demo and the value pod. Labels can be very useful for querying and selecting objects based on their purpose or characteristics.

- **spec:** This section describes the Pod's desired state:
  - **containers:** A list of containers that should be part of this Pod:
    - **name:** httpd: The name of the container, which is httpd in this case. This is how you can refer to this container within the Pod.
    - **image:** docker.io/httpd: The Docker image for the container. This specifies that the container should run the httpd server image from Docker Hub.
    - **imagePullPolicy:** IfNotPresent: This tells Kubernetes to pull the image only if it is not already present locally. This can speed up deployment if the image is already available on the node.
    - **ports:**
      - **containerPort:** 80: This specifies that the container listens on port 80. This information can be used by services and other networking resources to route traffic to this container.


## Apply and create the Pod

```
kubectl apply -f lco-pod-demo.yml
```
## Verify the status of Pod created

```
kubectl get pod
```

## Describe the pod 

```
kubectl describe lco-pod-demo-2
```

# How to login into pod

```
kubectl exec -it [POD_NAME] -- /bin/sh
```

# how to create multicontainer pod

```
apiVersion: v1
kind: Pod
metadata:
  name: my-multi-container-pod
  labels:
    purpose: demonstrate-multi-container
spec:
  containers:
  - name: my-nginx
    image: nginx:latest
    ports:
    - containerPort: 80

  - name: my-redis
    image: redis:alpine
    ports:
    - containerPort: 6379
```
# Create Pod

```
kubectl apply -f multicontainer.yml
```

# How to login in to a Pod if multiple container exist on it.

```
kubectl exec -it my-multi-container-pod -c redis-container -- /bin/sh
```









