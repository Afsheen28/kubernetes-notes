# Kubernetes Notes 
## Definition of Kubernetes
* Open source container orchestration tool.
* Developed by Google.
* Helps manage containers(or docker containers)/ containerised applications that are made up of hundreds or maybe thousands of containers.
* Helps you manage them in different environments like physical or virtual machines or even cloud technologies/ hybrid deployment environments.

## What problems does Kubernetes solve?



## What are the tasks of an orchestration tool?
* Trend from Monolith to Microservices.
* Increased usage of containers
* Demand for a proper way of managing those hundreds of containers
  
## What features do orchestration tools offer?
* High Availability or no downtime - always accessible by users
* Scalability or high performance - application has high performance meaning it loads fast and users have a very high response rate
* Disaster recovery - backup and restore
  
## Kubernetes Basic Architecture
* Made up of one master node and then connected to a couple of worker nodes.
* Each node has a kubelet (primary "node agent") process running on it and cubelet is actually a kubernetes process that makes it possible for the cluster to communicate to each other and carry out some tasks.  
* Each node has different docker applications deployed on it.
* Worker nodes are where the applications are running.
* The Master node carries out important Kubernetes processes running.
* One of these processes is the "API server" which is also a container.
* The "API server is an entrypoint to the kubernetes cluster. This is the process that different Kubernetes clusters will talk to. For example, UI, API, CLI.
* Another process running on the Master node is the "Controller Manager".
* The "Controller Manager" keeps track of what's happening in the cluster.
* Another process is the "Scheduler". This ensures Pods placement.
* Entire cluster is an etcd which is a key-value storage which holds the current state of the kubernetes cluster.
* The virtual network enables these nodes to talk to each other. It helps to create one powerful machine that has the sum of all the resources of individual nodes.
* Worker nodes are more bigger and have more resources due to the amount of workload they have and the hundreds of containers they will be running. 
* Master nodes doesn't carry out many processes compared to worker nodes but is much more important as if you end up loosing it, you won't be able to have access to the cluster anymore. In production, having an extra Master node is ideal so that you will have a backup for the cluster without completely loosing it.
* If you loose a worker node, you may be able to have another one created for you.

## Kubernetes Basic Concepts
* Pods are the smallest unit that we will use in Kubernetes.
* Is a wrapper of a container in each worker node.
* Each worker node has multiple pods and inside each pod, you can have multiple containers.
* Each application would have one pod. However, you could possibly have more than one pod inside a container when you have a main application that needs some helper containers.
* There will be one pod per application such as one pod for the database, one pod for the message broker, one pod for a server and so on.
* A node.js or Java application will also have one separate pod. 
* The virtual network controls the Kubernetes cluster that assigns each pod it's own IP address so each pod is its own self-contained server.
* The way they can communicate within each other is by using the interal IP addresses.
* We only work with the pods within the Kubernetes cluster which is an abstraction layer over containers and pod is a component of Kubernetes that manages the containers running by itself.
* Pods and the containers inside a pod are recreated frequently when they stop working. 
* When a pod stops working and a new one gets created, the new pod gets a new IP address on creation.
* When such a situation takes place, it gets very inconvenient in changing the IP address again and again. In order to substitute for this, another component is used named "Service". 
* This basically is an alternative to the IP addresses and so ensures that each pod can be communicated with. 
* Services have 2 main functionalities. One is a permanent IP address which can be used to communicate between the pods and at the same time is a load balancer.

## Kubernetes Configuration
* All configuration in the Kubernetes cluster goes through the Master node with the process called "API server".
* A Kubernetes' client could be the UI, Kubernetes Dashboard or an API which could be a script or a curl command or a command line tool (cube CTL), they all talk to the API server and they send their configuration requests to the API server which is the main entry point or the only entry point into the cluster.
* The requests should either be in YAML or JSON format. 
* Deployment = a template for creating pods.
* Configuration should be "declarative"  to declare what the desired outcome is.
* Is == Should.
* Controller Manager checks desired state == actual state