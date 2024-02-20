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


# Worker Node and its components

![image](https://github.com/awsbatch/my-k8s/assets/110165635/b28f37a3-1909-4376-a545-60e2e7492a7a)

#### Worker nodes listens to the API Server for new work assignments. They execute the work assignments and then report the results back to the Kubernetes Master node. Each worker node is controlled by the Master Node.

# Kubelet
- #### The kubelet runs on every node in the cluster. It is the principal Kubernetes agent. It interacts with etcd store to read configuration details and write values. It watches for tasks sent from the API Server, executes the task, and reports back to the Master. It also monitors pods and reports back to the control panel if a pod is not fully functional. Based on that information, the Master can then decide how to allocate tasks and resources to reach the desired state.
- #### Ensures that containers are running in a Pod on the node.
- #### The primary agent that runs on each worker node.
- #### Communicates with the Kubernetes control plane, specifically with the API server on the master node.


# Container Runtime
- #### The container runtime pulls images from a container image registry and starts and stops containers. A 3rd party software or plugin, such as Docker, usually performs this function.
- #### The software responsible for running containers, like Docker or containerd.
- #### Interacts with the underlying operating system to start, stop, and manage containers.

# Kube-proxy
- #### The kube-proxy makes sure that each node gets its IP address, implements local iptables and rules to handle routing and traffic load-balancing. This ensures that the necessary rules are in place on the worker nodes to allow the containers running on them to reach each other.
- #### Maintains network rules on nodes, enabling communication between Pods and external traffic.
- #### Enables communication between Pods on different nodes and allows external traffic to reach the Pods.


![image](https://github.com/awsbatch/my-k8s/assets/110165635/2531bcf0-b3ca-4c3a-980a-71cc3433cd0c)



# Kubernetes Objects

### In Kubernetes, objects are persistent entities that represent the state of the cluster. These objects describe the desired state of a cluster or its components and can be used to create, update, or delete resources. The following are some of the common types of objects in Kubernetes:

# Pod:
- The smallest deployable units in Kubernetes.
- Consists of one or more containers that share the same network namespace.

# Service:
- Abstracts the access to a set of Pods, providing a stable endpoint for communication.
- Types include ClusterIP, NodePort, LoadBalancer, and ExternalName.

# Volume:
- Provides data persistence for containers.
- Can be used by one or more containers in a Pod.

# Namespace:
- A way to divide cluster resources between multiple users (via resource quota), or to partition resources within a user.

# Deployment:
- Allows you to declaratively define and manage application deployments.
- Ensures a specified number of replica Pods are running at all times.

# ConfigMap:
- Enables you to separate configuration details from application code.
- Can be used to store non-sensitive configuration data.

# Secret:
- Stores sensitive information, such as passwords or API keys.
- Encoded or encrypted data that can be decoded by the Kubelet when mounting secrets into Pods. Learn more about Kubernetes secrets

# Ingress:
- Manages external access to services within a cluster.
- Acts as a layer 7 (application layer) HTTP load balancer.

# StatefulSet:
- Manages the deployment and scaling of a set of Pods, and provides guarantees about the ordering and uniqueness of these Pods.

#### These components work together to provide a robust and scalable container orchestration platform, allowing users to manage and scale containerized applications with ease. Understanding the role of each component is crucial for effective Kubernetes administration and application deployment.




  
