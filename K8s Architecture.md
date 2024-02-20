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

![image](https://github.com/awsbatch/my-k8s/assets/110165635/753b0fba-7412-4387-8126-e5270960fa0c)

- The Kubernetes Master receives input from CLI or UI via an API. These are the commands which you provide to Kubernetes. The master node is the main entry point for all administrative tasks you want to perform on the cluster. It controls all the worker nodes.

- You define pods, replica sets, and services that you want Kubernetes to maintain. For example what container image to use, which ports to expose, and how many pod replicas to run etc..

- You also provide the parameters of the desired state for the application(s) running in that cluster.

- In Kubernetes, a master node is a critical component of the cluster responsible for managing and controlling the overall state and configuration of the system. It is the brain of the Kubernetes cluster, handling the coordination of tasks and communication between various components. The master node oversees the entire orchestration process, ensuring that applications are deployed, scaled, and managed efficiently.

# Kube-API Server

- #### The Kube-API Server is the front-end of the control plane and the only component in the control plane that we interact directly with. Internal system components, as well as external user components, all communicate via the same API.

# Key-Value Store (etcd)

- #### The Key-Value Store, also called etcd, is a database Kubernetes uses to back-up all cluster data. It stores the entire configuration and state of the cluster. The Master node queries etcd to retrieve parameters for the state of the nodes, pods, and containers.

# Controller Manager

- #### The role of the Controller is to obtain the desired state from the API Server. It checks the current state of the nodes it is tasked to control, and determines if there are any differences, and resolves them, if any.
- #### The controller manager is responsible for managing the lifecycle of pods and other Kubernetes resources. The controller manager watches for changes to Kubernetes resources and takes action to ensure that the resources are in the desired state.
- #### The worker nodes are the physical or virtual machines that run the containers. Worker nodes are responsible for running pods and providing resources to the pods. Worker nodes consist of the following components:

# Scheduler

- #### A Scheduler watches for new requests coming from the API Server and assigns them to healthy nodes. It ranks the quality of the nodes and deploys pods to the best-suited node. If there are no suitable nodes, the pods are put in a pending state until such a node appears.
- #### The scheduler is responsible for scheduling pods to run on worker nodes. The scheduler takes into account the resources available on each node and the requirements of the pods when making scheduling decisions.

# Container runtime engine
- #### The container runtime engine is responsible for running the containers on the worker node. The most popular container runtime engine is Docker.

  
