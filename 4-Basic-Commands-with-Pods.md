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
