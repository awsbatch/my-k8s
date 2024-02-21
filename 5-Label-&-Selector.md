# <div align="center">Kubernetes Core Concepts - Labels, Selectors and Annotations</div>


![image](https://github.com/awsbatch/my-k8s/assets/110165635/445b6b3b-ca8a-4471-b99d-66f3bea70a3d)



# What are Labels in Kubernetes?

- As per official documentation Labels are key/value pairs that are attached to objects, such as pods, services, deployments and replicasets etc.

- When we categorize something based on their properties or characteristics and give them some name those are nothing but called as Labels.

![image](https://github.com/awsbatch/my-k8s/assets/110165635/8753ecfd-2bce-43fe-ba84-916f25b656b9)



![image](https://github.com/awsbatch/my-k8s/assets/110165635/c0b700f8-15e2-4efe-b91d-605eb320079a)




# K8s pods with Kubernetes labels indicating their environment.


![image](https://github.com/awsbatch/my-k8s/assets/110165635/1165d24d-d9c2-4e7d-b46f-75b0f38e2b9e)


# Kubernetes labels syntax and naming

#### Let us assume we have three sets of a same application running on three different environments such as- prod, dev and staging. In such cases we need to assign labels to those resources using their object configurations files.

```
# where environment is production
labels:
    environment: prod
    app: nginx
```

```
# where environment is development
labels:
    environment: dev
    app: nginx
```

```
# where environment is staging
labels:
    environment: staging
    app: nginx
```

We can attach Labels to objects during their creation time or can add or modify them later as well. Each Kubernetes object can have a set of key/value labels defined. Each label key must be unique for a given object.
