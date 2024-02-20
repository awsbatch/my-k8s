![image](https://github.com/awsbatch/my-k8s/assets/110165635/d8180936-592c-402c-bca3-769a58a41be3)

# What is Kubernetes?

#### It's an open source container orchestration platform that helps manage distributed, containerized applications at a massive scale. It was designed to run enterprise-class, cloud-enabled and web-scalable IT workloads.
#### Kubernetes has now become one of the hottest technology in the IT world and every enterprise today wants to leverage the same within their infrastructure.
#### Most of the companies if not all directly or indirectly have their workloads hosted over Kubernetes.

#### Kubernetes (commonly abbreviated as K8s) is an open-source container orchestration platform designed to automate the deployment, scaling, and management of containerized applications. Originally developed by Google, Kubernetes has become one of the most popular tools for managing containerized workloads and services.

# What can be achieved by using Kubernetes
* It provides auto-scalable containerized infrastructure
* It provides Application-centric management
* Highly available distributed system
* It is made to be portable, enable updates with near-zero downtime, version rollbacks.
* Clusters with ‘self-healing’ capabilities when there is a problem
* Load balancing, auto-scaling and SSL can easily be implemented
* Environment consistency across development testing and production
* Loosely coupled infrastructure, where each component can act as a separate unit

#### One of the key components of Kubernetes is, it can run application on clusters of physical, virtual and cloud infrastructure as well. It helps in moving from host-centric infrastructure to container-centric infrastructure.

# Kubernetes Architecture

#### Kubernetes follows client-server architecture. Wherein, we have master installed on one machine and the worker nodes on separate Linux machines.

![image](https://github.com/awsbatch/my-k8s/assets/110165635/e69da1b8-e790-4b93-897f-191310ee5969)

### Kubernetes has a decentralized architecture that does not handle tasks sequentially. It functions based on a declarative model and implements the concept of a desired state.

# Master Node (Control Plane) and its components
