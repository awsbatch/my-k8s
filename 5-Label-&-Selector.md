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

#### We can attach Labels to objects during their creation time or can add or modify them later as well. Each Kubernetes object can have a set of key/value labels defined. Each label key must be unique for a given object.


# Valid label values


#### When working with Kubernetes labels, there are restrictions concerning the length and allowed values. Specifically, a valid label value:

- must be 63 characters or less (can be empty),
- unless empty, must begin and end with an alphanumeric character ([a-z0-9A-Z]),
- can only contain dashes (-), underscores (_), dots (.), and alphanumerics

For example, the following pod manifest has two valid labels, environment: production and app: nginx

```
apiVersion: v1
kind: Pod
metadata:
  name: label-demo
  labels:
    environment: production
    app: nginx
  spec:
    containers:
    - name: nginx
      image: nginx:1.14.2
      ports:
      - containerPort: 80
```

## How to manage Kubernetes labels?

```
 kubectl get pods --show-labels
```

#### we can use the label subcommand to add another label to the pod.
```
kubectl label pods labelex owner=ravendra
```

####  we can use the --overwrite flag to change the value for the existing label key.

```
kubectl label pods labelex owner=rahul --overwrite
```

#### use the short option -l to select the pod with label env=develop.

```
kubectl get pods -l env=develop
```

#### We can now list the pods where the environment is either develop or prod by using the set-based selector. For example:

```
kubectl get pods -l 'env in (prod, develop)'
```

#### Other actions also support label selection. For example, we can delete both the pods using this command:

```
kubectl delete pods -l 'env in (prod, develop)'
```

### How to Remove labels
```
kubectl get pod --show-labels
NAME     READY   STATUS    RESTARTS   AGE   LABELS
my-pod   1/1     Running   0          10m   run=my-pod
```

```
kubectl label pod my-pod run-
pod/my-pod unlabeled
```

# What are Selectors in Kubernetes?

- Selectors are used to filter out objects based on their assigned Labels. Labels and Selectors goes hand in hand.

For example Selectors will help us filter out objects like give all the application pods which are of type staging.


# Example syntax to define Selectors

```
# where environment is production
selector:
    matchLabels:
      environment: prod
```

```
# where environment is development
selector:
    matchLabels:
      environment: dev
```

```
# where environment is staging
selector:
    matchLabels:
      environment: staging
```


# Selector Types

Kubernetes supports two type of selectors −

- Equality-based selectors
- Set-based selectors

### Equality-based Selectors ->

They allow filtering by key and value. Operators used as part of this are: =, ==, !=

#### Examples:

Here are few pods running in my environment. To view them by their Label name run the following command:

```
kubectl get pods --show-labels
```

```
kubectl get pods environment = prod   --show-labels  
```

```
kubectl get pods environment != prod   --show-labels  
```

### Set-based Selectors->

Set-based selectors allow filtering of keys according to a set of values. Operators used as part of this are: in, notin, exists.

### Examples:

To view all the pods with labels environment=prod and environment=staging.

```
kubectl get pods -l 'environment in (prod,staging)' --show-labels
```

```
kubectl get pods -l 'environment notin (staging)' --show-labels
```

To view all the pods with a key environment and value distinct from staging and all resources with no label with the key environment.
```
kubectl get pods -l 'environment notin (staging)' --show-labels
```
# matchExpressions Selector

matchExpressions label selector is more expressive in nature. It supports support set-based matching whereas matchLabels only supports exact matching. matchExpressionscan label selector can be used with or without the matchLabels selector.

```
# where environment is staging
selector:
    matchLabels:
      environment: staging
    matchExpressions:
      - {key: tier, operator: NotIn, values: [front-end]}
```

