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

## Kubernetes Manifest
Project based on a course in Linkedin Learning - Kubernetes: your first project. Use Kubernets Manifests and Kind to deploy an application locally. Kind is a tool that allows you to create Kubernetes custers locally inside of Docker.

### Kubernetes Manifests
Are files that are used to install and configure objects within a Kubernetes cluster. Kubernetes creates resources through YAML files, applications describing what you want to creat.

#### Creatin a Kubernetes Manifest
1. Running the command above create a YAML file with the image names and configurations and saves it into deployment.yml
```  
kubectl create deployment --dry-run=client --image localhost:5000/explorecalifornia.com explorecalifornia.com --output=yaml > deployment.yml
```

### Deploying the website into Kubernetes
1. Create the pods that will run the container for our website. Note that creating pods directly is not the same as creating deployments. Deployments make sure that the number of pods we want hosting out web site stay up and running all time.
2. Tag the image
```
docker tag explorecalifornia.com 127.0.0.1:5000/explorecalifornia.com
```
3. And push to the local registry.
```
docker push 127.0.0.1:5000/explorecalifornia.com
```
4. Apply the deployment.yml
5. Use kubectl port-forward to mount ports frm the host into the pods inside the deployment
```
kubectl port-forward deployment/explorecalifornia.com 8080:80
```

### Kubernetes Services
To make the website acessible we map explorecalifornia.com into a single point of entry that is going to live inside Kubernetes, the Service. 
1. Create a clusterip. That creates avirtual IP address that maps all the pods that are running inside of our deployment.
```
kubectl create service clusterip --dry-run=client --tcp=80:80 explorecalifornia.com --output=yml > service.yml
```
2. AQpplies the service.
``` 
kubectl apply -f service.yml
```
3. Verify if it is working.
```
kubectl port-foward service/explorecalifornia-svc 8080:8080
```

### Ingress
Enables external access into other kubernetes resources. 
1. Create an ingress
```
kubectl create ingress explorecalifornia.com --rule="explorecalifornia.com/=explorecalifornia-svc:80" --dry-run=client --output=yaml > ingress.yaml
```
2. Install nginx ingress controller.
```
kubectl apply -f https://raw.githubusercontent.com/kubernetes/ingress-nginx/main/deploy/static/provider/kind/deploy.yaml
```
3. Apply ingress
```
kubectl apply -f ingress.yml
```

## Microservices
Is a service-oriented architecture that structures the application as a colective of loosely coupled services. This part of the repo is based on the Linkeding Learning course - Kubernetes: Microservices. Starts with a monolith application based in three modules User Module (user managment, login and profile managment), Catalog Module (list of products), Wish List Module (API used by the customer).

### Twelve-Factor Methodology

#### Principle 1: Codebase
Codebase must be tracked in version control and will have many deploys.

#### Principle 2: Dependencies
Dependencies are explicity declared and isolated.

#### Principle 3: Configuration
Add configuration via environment variables or config files.

#### Principle 4: Backing Services
Treat backing services as an attached resource.

#### Principle 5: Build, Release and Run
Always have a build and deploy strategy. Build strategies for repeat builds, versioning of running system and rollback.

#### Principle 6: Processes
Execute application as a stateless process. 

#### Principle7: Port Bindings
Expose services via port bindings. 

#### Principles 8, 9 and 10:
Concurrency (Scale out with the process model), Disposability (Quick application startup and shutdown times) and a Dev/Prod Parity (Application is treated the same way in dev, staging and production).

#### Principles 11 and 12:
Log Management and Admin Tasks.

### Building Blocks Directory
Cover the Codebase, Dependencies, Dev/Staging/Production Parity and Admin Processes. 

#### Codebase Workflow
1. Push code to your source control
2. Automate build is kicked off to build and run tests against code.
3. Build container image; push to image repository.

### Deployment Patter Directory
Applies Application Configuration, Build/Release/Run, Processes and Port Bindindgs.

#### Application Configuration
There are two ways of doing application configuration in Kubernetes:
- configMaps: for generic information (metada, version). 
- Secrets: for sensitive data
This values are fed to the pods as Environment Variables or Files

### Runtime Patterns Directory
Baking Services, Features Around Concurrency, Disposability and Log Management

### Breaking the Application
The application is devides into three microservices (Authentication, Wish List Funtionality and The Catalog Operations). Each Microservice has its own REST API.
