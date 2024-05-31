

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


# In this article, you will learn:

- Kubernetes Storage Concepts
  - Container Storage Interface (CSI)
Volumes
Non-Persistent Storage
Persistent Volumes (PV) and Persistent Volume Claims (PVC)
StorageClasses
Dynamic Provisioning of StorageClasses
Kubernetes Storage Best Practices
Kubernetes Volumes Settings
Limiting Storage Resource Consumption
Resource Requests and Limits
