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
