![image](https://github.com/awsbatch/my-k8s/assets/110165635/a303acab-e99b-48cc-a030-0fef9b93b375)

# Kubernetes Storage : Concepts and Best Practices
### What is Kubernetes Storage?
Kubernetes is a free and open-source container orchestration platform. It provides services and management capabilities needed to efficiently deploy, operate, and scale containers in a cloud or cluster environment.

When managing containerized environments, Kubernetes storage is useful for storage administrators, because it allows them to maintain multiple forms of persistent and non-persistent data in a Kubernetes cluster. This makes it possible to create dynamic storage resources that can serve different types of applications.

If properly managed, the Kubernetes storage framework can be used to automatically provision the most appropriate storage to multiple applications, with minimal administrative overhead.

## How Does Kubernetes Storage Work?
The Kubernetes storage architecture is based on Volumes as a central abstraction. Volumes can be persistent or non-persistent, and Kubernetes allows containers to request storage resources dynamically, using a mechanism called volume claims.

## Volumes

Volumes are the basic entity containers use to access storage in Kubernetes. They can support any type of storage infrastructure, including local storage devices, NFS and cloud storage services. Developers can create their own storage plugins to support specific storage systems. Volumes can be accessed directly from pods or Persistent Volumes (defined below).

## Non-Persistent Storage

By default, Kubernetes storage is temporary (non-persistent). Any storage defined as part of a container in a Kubernetes Pod, is held in the host's temporary storage space, which exists as long as the pod exists, and is then removed. Container storage is portable, but not durable.

## Persistent Storage

Kubernetes also supports a variety of persistent storage models, including files, block storage, object storage, and cloud services belonging to these and additional categories. Storage can also be defined as a data service, commonly a database.Storage can be referenced directly from within a pod, but this violates the pod’s portability principles and is not recommended. Instead, pods should use Persistent Volumes and Persistent Volume Claims (PV/PVC) to define the storage requirements of their applications.

## Persistent Volumes (PV) and Persistent Volume Claims (PVC)

PV and PVC separate storage implementations from functionality and allow pods to use storage in a portable way. It also separates users and applications from storage configuration requirements.Administrators can define storage resources, together with their performance, capacity and cost parameters, in a PV. A PV also defines details like routes, IP addresses, credentials, and a lifecycle policy for the data. PVs are not portable between Kubernetes clusters. A PVC, on the other hand, is used by users or developers to describe the storage required by the application. They are portable and can be moved together with an application. Kubernetes identifies the storage available in the defined PV, and if it matches the requirements in the PVC, binds the PVC to that storage.The PVC can specify some or all of the storage parameters defined in the PV. If, for example, the PVC defines only capacity and storage tier, it can be bounded to a larger variety of PVs (any that meet those criteria).

## Deployments and Stateless Sets

Kubernetes provides a construct called a deployment, which comprises several cloned pods, which share the same PVC. This can lead to stability issues. A better option is to run pods as stateless sets, which allows you to clone PVCs between containers.


## StorageClasses

You can configure StorageClass and assign PVs to each one. A StorageClass represents one type of storage. For example, one StorageClass may represent fast SSD storage, while another can represent magnetic drives, or remote cloud storage. This allows Kubernetes clusters to configure various types of storage according to workload requirements.

A StorageClass is a Kubernetes API for setting storage parameters using dynamic configuration to enable administrators to create new volumes as needed. The StorageClass defines the volume plug-in, external provider (if applicable), and the name of the container storage interface (CSI) driver that will enable containers to interact with the storage device.

## Dynamic Provisioning of StorageClasses

Kubernetes supports dynamic volume configuration, allowing you to create storage volumes on demand. Therefore, administrators do not have to manually create new storage volumes and then create a PersistentVolume object for use in the cluster. When the user requests a specific type of storage, the entire process runs automatically.

The cluster administrator defines storage class objects as needed. Each StorageClass refers to a volume plugin, also called a provisioner. When a user creates a PVC, the provisioner automatically provisions a volume according to the required storage criteria.


# Types of Volumes :-
## Kubernetes Hostpath

Hostpath is one of the supported volume types in the Kubernetes Cluster, it is a file or directory from the nodes file system into the pod. Hostpath will mount a directory, which is present on the node and mounted inside the container

A hostPath Persistent Volume must be used only in a single-node cluster

Kubernetes will support hostpath in the multimode cluster, if you create a deployment set using the hostpath the container will create and start but the data will not sync in each container running in multiple hosts

A host path is a type of persistent volume this will mount a file or directory from the host node’s filesystem into your pod.

Advantages We can use the node-local file system as persistent volume storage. It is very useful for testing or development purposes. It can be referenced by Persistent Volume Claim or we can directly use it in the pod YAML file.


### 1. hostpath Volume: 
Mount a directory /var/local/dataas a volume from the host machine to /usr/share/nginx/html location of the nginx container of a pod.

![image](https://github.com/awsbatch/my-k8s/assets/110165635/704f9f50-0e01-4dfd-8bd4-4f6d39d9af73)

```
#pod definition file with a volume 
apiVersion: v1
kind: Pod
metadata:
  name: webserver
spec:
  containers:
  - image: nginx:latest
    name: nginx-container
    volumeMounts:
    - mountPath: /usr/share/nginx/html
      name: test-vol
  volumes:
  - name: test-vol
    hostPath:
      path: /var/local/data
      type: DirectoryOrCreate
```


