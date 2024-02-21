![image](https://github.com/awsbatch/my-k8s/assets/110165635/aedba413-dbf6-4e0b-867a-dbe7bc87ffb8)

# What are Kubernetes Namespaces?

- #### As per official documentation Kubernetes supports multiple virtual clusters backed by the same physical cluster. These virtual clusters are called namespaces.
- #### In Kubernetes (often abbreviated as K8s), "ns" stands for "namespace." A namespace is a way to divide cluster resources between multiple users (via resource quota), teams, or projects. It provides a scope for Kubernetes objects, such as pods, services, replication controllers, and others.
- #### In a simpler way Kubernetes namespaces helps an organization working on/with different projects, teams, or customers to share a common Kubernetes infrastructure.
- #### Namespaces are often used to create virtual clusters within a single physical cluster, allowing teams or projects to work independently without interfering with each other's resources. They provide isolation and partitioning of resources, making it easier to manage and organize applications and services within a Kubernetes cluster.

# Why do you need namespaces?

- #### When we have a large set of Kubernetes resources running on our cluster Kubernetes namespaces helps in organizing them and allows you to group them together based on the nature of the project. By doing so you have more control over them and can effectively manage them by using unit filters.

- #### For example you can have separate namespaces created for different application life cycle environments such as development, staging, production etc. for a project or an application.

![image](https://github.com/awsbatch/my-k8s/assets/110165635/df5119f6-f805-46d2-9dfc-90e7a11b6772)

#### Names of all the kubernetes resources are required to be unique within a namespace, but not across namespaces. And this feature of having same resource object names across namespaces comes handy in application life cycle environments we discussed above.

# default Namespace in Kubernetes

By design, when you provision a Kubernetes cluster it will start with a default namespace to hold the default set of Pods, Services, and Deployments used by the cluster. The objects part of this namespace will be part of no other namespaces.

# By default, Kubernetes provides three namespaces:

- ### default: The default namespace for objects that don't have a namespace specified.
- ### kube-system: The namespace for Kubernetes system objects, such as kube-dns, kube-proxy, and others.
- ### kube-public: This namespace is automatically created and is readable by all users. It is primarily used for resources that should be accessible to all users, such as cluster information.

# List all Namespaces part of a Kubernetes cluster->

```
root@kube-master:~# kubectl get namespaces
```

```
root@kube-master:~# kubectl describe namespaces default
```

# Create a new Namespace

### Like any other Kubernetes object Namespaces also can be created in two ways.
- Using command line tool kubectl
- Using yaml manifest file

```
root@kube-master:~# kubectl create namespace test-ns
```

## Create Namespace using a yaml manifest file->

Like any other Kubernetes resource Namespaces also can be created by applying a manifest file.

```
root@kube-master:~# cat namespace-demo.yml
apiVersion: v1
kind: Namespace
metadata:
  name: demo-namespace
```

## Apply the file and create the Namespace->
```
root@kube-master:~# kubectl apply -f namespace-demo.yml
```

## Launch kubernetes objects in newly created Namespace

```
root@kube-master:~# kubectl run namespace-demo-nginx --image=nginx --namespace=namespace-demo
```

## List the Pod created within that Namespace->

```
root@kube-master:~# kubectl get pods --namespace=namespace-demo
NAME                   READY   STATUS    RESTARTS   AGE
namespace-demo-nginx   1/1     Running   0          17s
```

