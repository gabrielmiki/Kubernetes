# Kubernetes
Pods are the smallest unit of compute. Kubernetes pods group similar containers into a logical unit. Containers inside a pod have the same IP addresses and move together. Pods get schedules on Kubernetes Kubelets Nodes
A cluster is an instance of Kubernetes. Each cluster has a control plane and at least one worker node. 
The control plane makes sure nodes and pods are created, modified and deleted without any issues. Made up of several components:
- Kube API Server: handles the most request form the user and inside the cluster.
- ETCD: key-value store. Saves all data about the state of the cluster.
- Kube Scheduler: identify nule created pods that have not been assiened to worker node and chooses a not for the pod to run on.
- Kube Controler Manager: checks the status of the cluster to make sure things are running properly.
- Cloud Controler Manager: conects the cluester with cloud providers' API.
A worker node is where pods are schedule and run, each worker node has three components:
- Kebelet: make sure that the containers ia a pod are running and healthy.
- Container Runtime
- Kube-proxy: certifies that your pods and services can communicate with other pods and services on nodes and in the control plane.
How a pod gets schedule on to a node:
1. Use of the command to apply a YAML file
2. Control Plane verifies the change and assign a node for the pod
3. Worker node checks the Kube API Server, verifies changes, pulls and starts the container

## What is Kubernetes
Kubernetes is a container Orchastretor. It makes decisions about where and how containerized applications are launched on the server, when to scale up or down the number of application replicas and what to do when an application or service stop working. 

### Kubernetes Namespaces
Namespaces let you isolate and organize workloads. Use different namespaces tp prganize the application and microsservices.

### Pods
Pods are the Kubernetes resource that run our applications and microsservices. 

### Kubernetes Service 
Services provides a single IP address and a DNS record for a group of related pods. There are three types of Kubernetes Service: LoadBalancer, ClusterIP, NotePort. 
- LoadBalancer: Is a load balancer that direct traffic from the internet to kubernetes pods. A load balancer has a public and static IP address. 

### CPU and Memory
The resources are related to the amount of memory and CPU a pod is using.

### Kubernetes Deployment
Kubernets depl0yment is the most cummon way to deply containerized application. Provides a desired state for pods in an application. Deployments allows you to controll the number of replicas running. In orther to deploy a new application, Kubernetes can keep the old version up and running, roll up the new version, make sure that the new pods are running and healthy and than remove the old pods.

### Kebernetes DaemonSet
Another way to deploy pods. Puts one copy of a container on every node running in the cluster, can not controll the number of replicas running. 

### Kebernetes Job
Kubernetes job also allows you to deploy more one pod at a time. A job will create one or more pods and run the container insede of them until it has successfully complete its task.

### Data Storage
The handle storage in Kubernetes can be done in some ways such as:
- Connect your application with a Database running outside the cluster.
- Use a Kubernetes Persistent Volume.

### Kubernetes Security
A more secutiy environment can be set through the adition of a security context info to the pods.
``` 
securityContext:
  allowPrivilegeEscalation: false
  runAsNonRoot: true
  capabilities:
    drop:
      - ALL
  readOnlyRootFilesystem: true
```
This way the container is run as non root (can not use sudo to run escallation commands) and filesystem is read only.
Other way to ensure security is to scan YAML files with a tool.

### Kubernetes Commands
- kubectl cluster-info (cluster settings)
- kubectl get nodes (visualize the nodes)
- kubectl get namespaces (namespaces created)
- kubectl get pods -A (services installed when a cluster is spinned up)
- kubectl get services -A (services running in a cluster) 
- kebectl apply -f namespaces.yml (way to create new namespaces)
- kubectl delete -f namespaces.yml (way to delete namespaces)
- kubectl get deplyments -n namespace_desired
- kubectl get pods -n namespace_desired
- kubectl describe pod name_pod -n namespace_desired
- kubectl get pods -n namespace_name -o wide (IP addresses of our pods)
- kubectl exec -it pod_name -- /bin/sh

## First Project
Use Kubernets and Kind to deploy an application locally. Kind is a tool that allows you to create Kubernetes custers locally inside of Docker.

### Kubernetes Manifests
Are files that are used to install and configure objects within a Kubernetes cluster.

### Commands Steps
...
5. Create a local registry with:
```
docker run --name local-registry -d --restart=always -p 5000:5000 registry:2
```
6. Verify the existance of the registry:
```
curl http://localhost:5000/v2
```
7. Verify local-registry content:
```
curl --location http://localhost:5000/v2
```
