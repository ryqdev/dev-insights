# Kubernetes Tips

## Kubernetes

K8s is an open-source technology for automating, managing, and scaling containers.

### Basic

![](https://raw.githubusercontent.com/Yukun4119/BlogImg/main/ubuntu\_img/20220418163852.png)

**Pod:**

* Smallest unit of K8s
* Abstraction over container
* Usually 1 application per Pod
* Each Pod gets its own ID address
* New IP address on re-creation

**Service:**

* Permanent IP address
* lifecycel of Pod and Service NOT connected

**Ingress:**

* The external service

#### ConfigMap and Secret

**Pods communicate with each others using Services**

**ConfigMap:**

* External configuration of your application
* Don't put credentials into ConfigMap (for security reasons)

**Secret:**

* used to store secret data
* base64 encoded
* The built-in security mechanism is not enabled by default

**Volumes**

* Storage on local machine
* or remote, outsite of the k8s cluster
* K8s doesn't manage data persistance

#### Deployment and Statful Set

What if my-app dies? We need replicate everything!

Service has 2 functionalities:

1. permanent IP
2. load balancer

**Deployment**

* blueprint for pods
* you create Deployments
* abstraction of Pods
* DB can't be replicated via Deployment (Since DB has states replication may lead to data inconsistent)
* DB are often hosted outside of K8s cluster

**StatfulSet**

* for STATEFUL apps: elastic, mongoDB, MySQL

### Kubernetes Architecture

Master node and worker nodes

**Node processes**

* each node has multiple Pods on it
* 3 processes must be installed on every Node (Kubelet, Kube Proxy and Container runtime)
* Worker Nodes do the actural work
* Kubelet interacts with both - the container and node

**Master processes**

* 4 processes run on every master node (Api server, Scheduler, Controller manager, and etcd

**Example Cluster Set-up**

2 Master nodes (less resources) and 3 worker nodes (more resources)

* Add new master/node server:
    1. get new bare server
    2. install all the master/worker node processes
    3. add it to the cluster

#### Minikube

Master and worker processes run on ONE machine (for local test)

* create Virtual Box in you laptop
* Node runs in that Virtual Box
* 1 Node K8s cluster
* for testing purposes

#### Kubectl

The most powerful of 3 clients. Kubectl can be just a minikube

* create pods
* create services
* destory pods

#### yaml file in K8s

example:

```yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: nginx-deployment
  labels:
    app: nginx
spec:
  replicas: 2
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
        image: nginx:1.16
        ports:
        - containerPort: 8080
---
apiVersion: v1
kind: Service
metadata:
  name: nginx-service
spec:
  selector:
    app: nginx
  ports:
    - protocol: TCP
      port: 80
      targetPort: 8080
```

Every configuration file has 3 parts:

1. metadata
2. sepcification (spec)
3. status (k8s will acuomatically compare the spec of configuration file and added to cluster)

### Basic Kubernetes commands

#### CRUD commands

* Create deployment

```shell
kubectl create deployment [name]
```

* Edit deployment

```shell
kubectl edit deployment [name]
```

* Delete deployment

```shell
kubectl delete deployment [name]
```

#### Statues of different K8s components

```shell
kubectl get nodes | pod | services | replicaset | deployment
```

#### Debugging pods

```shell
kubectl logs [pod name]
kubectl exec -it [pod name] -- bin/bash
kukectl describe pod [pod name]
```

#### Use configuration file for CRUD

```shell
kubectl apply -f [file name]
kubectl delete -f [file name]
```

#### Set configuration

```shell
kubectl [do something] --kubeconfig==[path to yaml]
```

### How to use K8S

#### Install Minikube

[https://minikube.sigs.k8s.io/docs/start/](https://minikube.sigs.k8s.io/docs/start/)

Minikube has kubectl as dependency, so kubectl will be installed automatically when we install Minikube

```shell
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
sudo install minikube-linux-amd64 /usr/local/bin/minikube
```

Minikube dependes on drives such as [Docker](https://minikube.sigs.k8s.io/docs/drivers/docker/) and [Hyperkit](https://minikube.sigs.k8s.io/docs/drivers/hyperkit/).

#### Install kubectl on Linux

[https://kubernetes.io/docs/tasks/tools/install-kubectl-linux/](https://kubernetes.io/docs/tasks/tools/install-kubectl-linux/)

### Reference

[https://kubernetes.io/](https://kubernetes.io/)

[https://computingforgeeks.com/deploy-kubernetes-cluster-on-ubuntu-with-kubeadm/](https://computingforgeeks.com/deploy-kubernetes-cluster-on-ubuntu-with-kubeadm/)

[https://www.youtube.com/watch?v=X48VuDVv0do](https://www.youtube.com/watch?v=X48VuDVv0do)

