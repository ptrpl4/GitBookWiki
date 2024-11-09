# ⚙️ Kubernetes

Also known as k8s, is an open-source system for automating deployment, scaling, and management of containerized applications. 

"Cluster management system".

#### links

- [slurm io course (ru)](https://www.youtube.com/playlist?list=PL8D2P0ruohOBSA_CDqJLflJ8FLJNe26K-)
- [doc](https://kubernetes.io/docs/concepts/)

### Managed Kubernetes

Environment where cloud provider is responsible for managing the control plane, which includes the Kubernetes API server, scheduler, and controller manager.

Examples
- Google Kubernetes Engine (GKE)
- AWS Fargate
- Amazon Elastic Container Service for Kubernetes (EKS)
- Azure Kubernetes Service (AKS)
- IBM Cloud Kubernetes Service

## List of Components

![](../../aaa-assets/k8s-1.png)

* Node or "Worker Node" - virtual or physical machine
* Master Node - Main Node for cluster
* Pod - abstraction over container. Usually 1 Pod = 1 App
* Service - static address for Pod (not connected with Pod lifecycle)
* Ingress - routs traffic to k8s cluster and sends to services inside k8s
* ConfigMap - keeps external config of application. could keep env. vars
* Secret - same as config map, encoded in base64 + should be secure encoded
* Volume - storage for data local (inside Node) or remote
* Deployment - 'blueprint' for Pods configuration
* StatefulSet 'sts' - component for stateful apps or DB
* DaemonSet

## Architecture

K8s cluster contains of:

* At least one **Master Node** (virtual or physical machine)
* Multiple **Worker Nodes** with **kubelet** process ("node agent") on it.

**kubelet** - allows running application processes and communicate

**Apps** are running on Worker Nodes in containers inside Pods (typically pod=container), controlled by ReplicaSet, controlled by Deployment.

### Master Node

On Master Node are running **Main k8s Processes** to run and manage cluster. Better to use multiple.

#### Master Node Processes

![](../../aaa-assets/k8s-2.png)

* API Server entrypoint to cluster
  * Access through **UI/API/CLI**
* Controller Manager
  * Monitoring what's going on in cluster
* Scheduler
  * controls the placement of the pods depending on the load
* etcd
  * key-value storage\
    storing info and configs. etcd snapshots are using for backup
  * makes snapshots for recovering proccees

### Pod

Pod is the basic execution unit of a containerized application, represents a single instance of a running process in cluster. 

It's a logical host for one or more containers. 

![](../../aaa-assets/k8s-3.png)

`pod.yaml`

```yaml
--- # begining of first document
apiVersion: v1
kind: Pod
metadata:
  name: my-pod
spec:
  containers: # list of containers. coulde be more
  - image: quay.io/testing-farm/nginx:1.12
    name: nginx
    ports: # mostly for documentation
    - containerPort: 80
... # ending of first document
```

### Service

Could be internal or external.

Has permanent ip address and works as load balancer if there is more than one Pod

![](../../aaa-assets/k8s-4.png)

### Config Map and Secrets

![](../../aaa-assets/k8s-5.png)

### Volume

![](../../aaa-assets/k8s-6.png)

Stores data. Could be local or remote

### Virtual Network

Virtual Network provides connection between nodes. "Creates one unified machine". Each Pod get internal IP address. If Pod will be restarted - IP will be chanegd.

### ReplicaSet

Kubernetes object that ensures a specified number of replicas (identical copies) of a Pod are running at any given time

- The ReplicaSet controller continuously monitors the number of replicas and ensures that the desired number is running
- If a Pod is deleted or becomes unavailable, the ReplicaSet controller creates a new replica to replace it
- if template data will be updated when replicaset is already running - changes will be applied only on new created pods, when they will be created

```yaml
---
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: my-replicaset
spec:
  replicas: 2
  selector:
    matchLabels:
	      app: my-app
  template:
    metadata:
      labels:
        app: my-app # way to mark related pods
    spec:
      containers:
      - image: quay.io/testing-farm/nginx:1.12
        name: nginx
        ports:
        - containerPort: 80
...
```

### Deployment

- doc - [https://kubernetes.io/docs/concepts/workloads/controllers/deployment](https://kubernetes.io/docs/concepts/workloads/controllers/deployment/)

- Higher-level abstraction that manages the deployment. It creates Pods using ReplicaSet.
- Provides a way to define and manage the lifecycle of applications in a declarative manner

![](../../aaa-assets/k8s-9.png)

### Service

- doc - [https://kubernetes.io/docs/concepts/services-networking/service/](https://kubernetes.io/docs/concepts/services-networking/service/)

![[k8s-10.png]]

## Configutations

Consist of

* metadata
* specification
* status (gets from **etcd**)

File format - .yaml, .json

#### Deployment file

`controllers/nginx-deployment.yaml`

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
  labels:
    app: nginx
spec:
  replicas: 3
  selector:
    matchLabels:
      app: nginx
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.14.2
        ports:
        - containerPort: 80
```

`/mongo.yaml`

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: mongo-deployment
  labels:
    app: mongo
spec:
  replicas: 1
  selector:
    matchLabels:
      app: mongo
  template:
    metadata:
      labels:
        app: mongo
    spec:
      containers:
      - name: mongodb
        image: mongo:5.0
        ports:
        - containerPort: 27017
---
apiVersion: v1
kind: Service
metadata:
  name: mongo-service
spec:
  selector:
    app.kubernetes.io/name: mongo
  ports:
    - protocol: TCP
      port: 8080
      targetPort: 27017
```

#### ConfigMap file

doc - [https://kubernetes.io/docs/concepts/configuration/configmap](https://kubernetes.io/docs/concepts/configuration/configmap/)

/mongo-config.yaml

```yaml
apiVersion: v1
kind: ConfigMap
metadata:
  name: mongo-config
data:
  mongo-url: mongo-service
```

#### Secret file

doc - [https://kubernetes.io/docs/concepts/configuration/secret](https://kubernetes.io/docs/concepts/configuration/secret)

/mongo-secret.yaml

```bash
# create example user and encode to base64
echo -n mongouser | base64
echo -n mongopassword | base64
```

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: mongo-secret
type: Opaque
data:
  mongo-user: bW9uZ291c2Vy
  mongo-password: bW9uZ29wYXNzd29yZA==
```

### Signal Terminate

`SIGTERM` - when Pod is terminated, Kubernetes sends a `SIGTERM` signal to the containers running in that Pod. This is part of the graceful shutdown process, allowing applications to clean up resources and finish ongoing requests before they are forcibly terminated.

k8s provides a grace period during which the container can handle the `SIGTERM` signal. By default - 30 seconds, can be configured in `terminationGracePeriodSeconds` field in the Pod specification.

App should know what to do on `SIGTERM`. 

`SIGKILL` will be sent to app when timer ends.

## Tools and Apps

### minikube

Tool for running and testing k8s locally. Emulates a cluster on a single machine

- https://github.com/kubernetes/minikube

![](../../aaa-assets/k8s-7.png)

Master node processes and Node processes inside one Virtual Node on your machine.

minikube works as container (docker, etc) or as virtual machine (virtualbox, etc)

is mainly used to start/delete a local cluster. kubectl gets configured to access the kubernetes cluster control plane inside minikube when the minikube start command is executed

#### commands

```bash
# installation
brew install docker
open -a docker
brew install minikube

# management
minikube start --driver docker # and define driver
minikube pause
minikube unpause
minikube stop

minikube delete --all

# check current status
minikube status

# run dashboard
minikube dashboard

# check addons
minikube addons list
```

### **kubectl**

![](../../aaa-assets/k8s-8.png)

**kubectl** - Kubernetes command-line tool. to have CLI access to cluster API Server, autoinstalled with minikube

#### commands

`create vs apply` - `create` will work only once, `apply` will add changes to current config (if any)

```bash
# add autocomplete
source <(kubectl completion zsh)  

# get active nodes
kubectl get node
# get created pods
kubectl get pod
kubectl get po -A # get all pods
kubectl get pods -l app=my-app # get by label
# get pods, deployments, services
kubectl get all

# get pod info
kubectl describe pod my-pod-name

# delete
kubectl delete replicaset --all
kubectl delete pods --all

# explain 
kubectl explain pod
kubectl explain pod.spec

# ReplicaSet
## create ReplicaSet
kubectl create -f replicaset.yaml
## add new changes
kubectl apply -f replicaset.yaml
## get
kubectl get rs # or full replicaset
## scale (also could be changed in .yaml and re-applied)
kubectl scale --replicas 5 replicaset my-replicaset # deleted will be newest pods
# change image in replica template 
kubectl set image replicaset my-replicaset 'nginx=quay.io/testing-farm/nginx:1.12'
```
